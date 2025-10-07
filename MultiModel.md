# 多模态进行疾病预测

好——下面给你一份实用、可操作的多模态建模指南（从数据准备到训练、评估、上线、可解释性与合规性），并附带一个**可运行的 PyTorch 模型骨架**（把基因组/蛋白组当做数值/向量特征或序列特征，临床文本用预训练 Transformer），可直接用作起点并按你数据细化。说明里会包含常见陷阱和工程实践（缺失模态、样本不均衡、批次效应、模型校准等）。
**注意（安全与合规）**：任何用于临床决策的模型都必须进行严格的临床验证和伦理/合规审查；下面仅为工程/研发指导，不构成医疗建议。

---

# 1) 高层流程（端到端概要）

1. 数据对齐与标注：按病人 ID 将三类模态（基因组、蛋白组、临床文本）对齐，定义预测目标（binary/multi-class/time-to-event）。
2. 预处理：清洗、缺失处理、批次/平台校正、标准化、tokenize 文本。
3. 模态编码器：为每种模态设计或选择合适的 encoder（例如：表型/omics 用 MLP/1D-CNN/变换器，文本用 ClinicalBERT）。
4. 融合策略：选择 early / hybrid / late fusion（见下文）。加上处理缺失模态的机制（模态掩码、imputation、modality dropout）。
5. 损失与训练：分类/回归/生存损失，可多任务。处理类不平衡（加权/ focal loss /过采样）。
6. 评估：恰当指标（AUROC、AUPRC、sensitivity/ specificity、calibration、CI、决策曲线分析、外部验证）。
7. 可解释性：SHAP、integrated gradients、attention visualization + 基因/蛋白的生物学解释。
8. 部署与合规：审计日志、可重复训练环境、隐私保护（去标识、访问控制、差分隐私或联邦学习）与临床验证流程。



# 2) 数据准备细节（关键要点）

* **对齐**：确保三模态有相同的 patient_id；如果有时间序列（多次测量），需决定窗口/汇总策略或使用时间模型（RNN/Transformer/time-aware attention）。
* **基因组**：

  * 如果是 variant 列表/VCF：可把每个样本映射为预选位点的二值/频率/注释向量，或提取功能注释（SIFT/PolyPhen/基因）、基因集富集分数等。
  * 如果是全基因表达（RNA-seq）：做 TPM/FPKM -> log1p -> 标准化，或选差异表达/基因集作为特征。
  * 批次效应：ComBat / limma / removeBatchEffect。
* **蛋白组**（蛋白表达/质谱强度）：缺失值多，考虑：缺失指示符 + KNN/low-rank/feature-wise imputation；log-transform；批次校正。
* **临床文本**（病历/出院小结）：去 PHI -> 用临床预训练模型分词（BioClinicalBERT、ClinicalBERT，或更大模型如 PubMedBERT）。可抽取概念（NegEx、cTAKES）作为辅助特征。
* **样本不均衡**：统计阳性/阴性分布，准备策略（重采样、class weights、focal loss）。
* **数据分割**：按病人分割 train/val/test，尽量做时间切分与外部验证集；使用交叉验证并保证分层（stratified K-fold）。



# 3) 模型与融合策略（权衡与建议）

常见方案与适用场景：

A. **Late fusion（独立编码器 + 预测层集成）**

* 各模态单独训练 encoder 输出表征（vector），再 concatenate 到一个 MLP 或加权 ensemble 进行分类。
* 优点：模块化、易解释、处理缺失模态容易（若某模态缺失可只用其余模态）。
* 适用：模态差异大、样本量有限时首选。

B. **Early / joint fusion（在 encoder 后早期拼接，然后联合 fine-tune）**

* 在编码后把表示拼接并通过 Transformer / MLP 联合学习交叉模态交互。
* 优点：能学习模态间复杂相互作用，提高性能（需更多数据/正则化）。

C. **Cross-attention / Multimodal Transformer**

* 以文本作为主干，使用 cross-attention 层将 omics features 投影为 tokens 并与文本交互（或相反）。
* 适用：当希望模型显式建模“文本中某句子与某蛋白/基因”的关联时。复杂但效果潜力高（数据量需求高）。

D. **Mixture-of-experts / Gating**

* 学习一个 gating 网络决定每个模态对最终预测的贡献（对缺失/噪声健壮）。

E. **Contrastive or multimodal pretraining**

* 若有大规模未标注数据，可先做跨模态对比学习（SimCLR/CLIP 风格）再下游微调，能显著提升少样本性能。



# 4) 处理缺失模态（实用技巧）

* 在训练中做 **modality dropout**（随机遮掉某模态）以训练鲁棒模型。
* 增加 **模态存在标志位**（one-hot）输入给融合层。
* 对数值模态做 imputation + 加上 imputed flag。
* 如果缺失模式不是随机（MNAR），考虑专门建模缺失机制或分层处理。



# 5) 评价指标（临床常用）

* 二分类：AUROC, AUPRC（当阳性稀少时 AUPRC 更重要），sensitivity@specificity, F1, calibration (Brier score, reliability curve)。
* 生存预测：C-index、time-dependent AUC。
* 报告：95% CI（bootstrap），决策曲线分析（net benefit）。
* 外部验证：在独立 cohort 上验证性能和漂移。



# 6) 可解释性与生物学验证

* **全局解释**：SHAP (tree/ deep explainer)、feature importance 排序。
* **局部解释**：Integrated Gradients、LIME。
* **文本解释**：可视化 attention scores 或 gradient-based highlight 关键句子。
* **生物学验证**：查询基因/蛋白是否出现在已知通路或文献中（KEGG/Reactome/PubMed）。



# 7) 工程细节与训练技巧

* **标准化**：数值模态使用训练集均值/方差；保存 scaler。
* **批次效应/平台差异**：在训练/验证分层地控制批次，或用域自适应方法。
* **正则化**：dropout, weight decay, early stopping。
* **优化与学习率**：AdamW，分类头用较高 lr，预训练文本 encoder 用较小 lr（参数分组）。
* **监控**：使用 TensorBoard / Weights & Biases 跟踪指标、模型版本。
* **模型选择**：在 validation 上用 AUROC/AUPRC 做选取，并保留最优阈值。
* **统计显著性**：对比基线模型（仅临床变量/仅基因组）并做统计检验（DeLong test for AUROC）。



# 8) 隐私、法规、部署

* 去识别化 PHI；保存最小必要数据。
* 若多中心数据敏感，考虑 **联邦学习** 或差分隐私训练（DP-SGD）。
* 部署：将模型容器化（Docker），暴露 REST API，记录模型版本与输入特征哈希以便审计。
* 临床部署前需临床试验/批准与伦理委员会审核。



# 9) 推荐的起步模型架构（实用 PyTorch skeleton）

下面给出一个**可运行的 PyTorch skeleton**：

* 文本用 HuggingFace Transformer（需安装 `transformers`）。
* Omics（基因/蛋白）当作数值向量用简单 MLP 编码。
* 融合采用 early fusion（concatenate → MLP）。
* 包含 modality dropout、class weight、训练与评估函数。

> 下面代码为骨架，需根据你数据 shape（omics feature 数）调整 `OMICS_DIM` 和 batch dataset 制作。

```python
# multimodal_model.py
import os
import math
import random
from typing import Dict
import torch
import torch.nn as nn
from torch.utils.data import Dataset, DataLoader
from transformers import AutoModel, AutoTokenizer
from sklearn.metrics import roc_auc_score, average_precision_score

# -------------------------
# Config / Hyperparams (adjust)
# -------------------------
TEXT_MODEL_NAME = "emilyalsentzer/Bio_ClinicalBERT"  # or "bert-base-uncased"
OMICS_DIM = 1024   # number of numeric omics features per sample (adjust)
HIDDEN_DIM = 256
BATCH_SIZE = 16
LR = 2e-5
EPOCHS = 10
MODALITY_DROPOUT_P = 0.2  # during training randomly drop a modality
DEVICE = "cuda" if torch.cuda.is_available() else "cpu"

# -------------------------
# Dataset skeleton (implement loading + preprocessing)
# -------------------------
class MultiModalDataset(Dataset):
    """
    Each item should be a dict:
      {
        "patient_id": ...,
        "text": "clinical note ...",
        "omics": np.array([...]) or torch.tensor,
        "label": 0/1
      }
    """
    def __init__(self, records):
        self.recs = records
        self.tokenizer = AutoTokenizer.from_pretrained(TEXT_MODEL_NAME)
    def __len__(self): return len(self.recs)
    def __getitem__(self, idx):
        r = self.recs[idx]
        text = r.get("text","")
        toks = self.tokenizer(text, truncation=True, padding='max_length', max_length=256, return_tensors="pt")
        omics = torch.tensor(r.get("omics", [0.0]*OMICS_DIM), dtype=torch.float32)
        label = torch.tensor(r["label"], dtype=torch.float32)
        return {"input_ids": toks["input_ids"].squeeze(0),
                "attention_mask": toks["attention_mask"].squeeze(0),
                "omics": omics,
                "label": label,
                "patient_id": r.get("patient_id")}

# -------------------------
# Model
# -------------------------
class TextEncoder(nn.Module):
    def __init__(self, model_name=TEXT_MODEL_NAME, out_dim=HIDDEN_DIM):
        super().__init__()
        self.backbone = AutoModel.from_pretrained(model_name)
        hidden_size = self.backbone.config.hidden_size
        self.project = nn.Linear(hidden_size, out_dim)
    def forward(self, input_ids, attention_mask):
        out = self.backbone(input_ids=input_ids, attention_mask=attention_mask)[0]  # last_hidden_state
        pooled = out[:,0,:]  # [CLS]
        return self.project(pooled)

class OmicsEncoder(nn.Module):
    def __init__(self, in_dim=OMICS_DIM, out_dim=HIDDEN_DIM):
        super().__init__()
        self.net = nn.Sequential(
            nn.Linear(in_dim, in_dim//2),
            nn.ReLU(),
            nn.BatchNorm1d(in_dim//2),
            nn.Linear(in_dim//2, out_dim),
            nn.ReLU()
        )
    def forward(self, x):
        return self.net(x)

class MultiModalClassifier(nn.Module):
    def __init__(self, text_dim=HIDDEN_DIM, omics_dim=HIDDEN_DIM, hidden=128):
        super().__init__()
        self.text_enc = TextEncoder(out_dim=text_dim)
        self.omics_enc = OmicsEncoder(in_dim=OMICS_DIM, out_dim=omics_dim)
        self.fusion = nn.Sequential(
            nn.Linear(text_dim + omics_dim + 2, hidden), # +2 for modality presence flags
            nn.ReLU(),
            nn.Dropout(0.2),
            nn.Linear(hidden, 1)
        )
    def forward(self, input_ids, attention_mask, omics, modality_mask=None):
        """
        modality_mask: dict {"text": bool, "omics": bool} indicating presence
        """
        text_feat = self.text_enc(input_ids, attention_mask) if modality_mask.get("text", True) else torch.zeros((omics.size(0), HIDDEN_DIM), device=omics.device)
        omics_feat = self.omics_enc(omics) if modality_mask.get("omics", True) else torch.zeros((omics.size(0), HIDDEN_DIM), device=omics.device)
        # modality presence flags
        text_pres = (1.0 if modality_mask.get("text", True) else 0.0)
        omics_pres = (1.0 if modality_mask.get("omics", True) else 0.0)
        flags = torch.tensor([text_pres, omics_pres], device=omics.device).unsqueeze(0).repeat(omics.size(0),1)
        h = torch.cat([text_feat, omics_feat, flags], dim=1)
        logits = self.fusion(h).squeeze(-1)
        return logits

# -------------------------
# Training & evaluation helpers
# -------------------------
def train_epoch(model, dataloader, opt, epoch):
    model.train()
    total_loss = 0.0
    criterion = nn.BCEWithLogitsLoss()  # could add pos_weight for imbalance
    for batch in dataloader:
        input_ids = batch["input_ids"].to(DEVICE)
        attention_mask = batch["attention_mask"].to(DEVICE)
        omics = batch["omics"].to(DEVICE)
        labels = batch["label"].to(DEVICE)
        # random modality dropout
        modality_mask = {"text": True, "omics": True}
        if random.random() < MODALITY_DROPOUT_P:
            modality_mask["text"] = False
        if random.random() < MODALITY_DROPOUT_P:
            modality_mask["omics"] = False
        logits = model(input_ids, attention_mask, omics, modality_mask)
        loss = criterion(logits, labels)
        opt.zero_grad()
        loss.backward()
        opt.step()
        total_loss += loss.item() * labels.size(0)
    return total_loss / len(dataloader.dataset)

def evaluate(model, dataloader):
    model.eval()
    ys, ps = [], []
    with torch.no_grad():
        for batch in dataloader:
            input_ids = batch["input_ids"].to(DEVICE)
            attention_mask = batch["attention_mask"].to(DEVICE)
            omics = batch["omics"].to(DEVICE)
            labels = batch["label"].cpu().numpy()
            modality_mask = {"text": True, "omics": True}  # use full modalities during eval
            logits = model(input_ids, attention_mask, omics, modality_mask)
            probs = torch.sigmoid(logits).cpu().numpy()
            ys.extend(labels.tolist())
            ps.extend(probs.tolist())
    auc = roc_auc_score(ys, ps) if len(set(ys))>1 else float("nan")
    aupr = average_precision_score(ys, ps) if len(set(ys))>1 else float("nan")
    return {"auc": auc, "aupr": aupr}

# -------------------------
# Example train loop (pseudo)
# -------------------------
def run_training(train_records, val_records):
    train_ds = MultiModalDataset(train_records)
    val_ds = MultiModalDataset(val_records)
    train_dl = DataLoader(train_ds, batch_size=BATCH_SIZE, shuffle=True)
    val_dl = DataLoader(val_ds, batch_size=BATCH_SIZE, shuffle=False)
    model = MultiModalClassifier().to(DEVICE)
    optimizer = torch.optim.AdamW(model.parameters(), lr=LR)
    best_auc = -1
    for epoch in range(EPOCHS):
        tr_loss = train_epoch(model, train_dl, optimizer, epoch)
        metrics = evaluate(model, val_dl)
        print(f"Epoch {epoch}: loss {tr_loss:.4f} val_auc {metrics['auc']:.4f} val_aupr {metrics['aupr']:.4f}")
        if metrics["auc"] > best_auc:
            best_auc = metrics["auc"]
            torch.save(model.state_dict(), "best_multimodal.pt")
    return model

# -------------------------
# Note: you'll need to prepare `train_records` and `val_records` lists of dicts as dataset input
# -------------------------
```

这段骨架包含关键工程元素：文本 encoder（可用 pretrained transformer），omics encoder（MLP），拼接融合，modality dropout。**在真实工程里你应当**：

* 调整 tokenizer/truncation、batch size（显存受限时减小），并把 Omics 特征维度与真实数据对齐；
* 加入 class weights、early stopping、LR 调度器、混合精度训练（`torch.cuda.amp`）以加速；
* 在验证/测试中禁用 modality dropout 并使用完整数据；在训练时用随机遮盖提高鲁棒性。



# 10) 进阶：若你的数据量较大或模态复杂

* 使用 **cross-attention** 层或 multimodal transformer（比如将 omics 特征视为 tokens）来捕捉细粒度交互。
* 采用 **contrastive pretraining**：把文本与 omics 对作为正样本，非对应病人作为负样本训练共享表征（CLIP 风格）。
* 如果有 time-series（多时间点），用 time-aware transformer / temporal convolution 网络。



# 11) 实验设计与报告建议

* 记录并报告：训练/验证/测试的 cohort 来源、时间范围、设备/测序平台、预处理流水线、超参、随机种子、版本化模型。
* 做**子群性能分析**（按年龄、性别、种族、批次）检测偏差。
* 报告模型不确定性与校准（置信区间、置信分数）。



# 12) 最后给你一组实用 checklist（做研发时跟着做）

* [ ] patient_id 对齐与去重完成
* [ ] 所有模态缺失率统计、分布可视化
* [ ] 批次效应检测与校正策略就绪
* [ ] train/val/test 严格分割（外部验证集）
* [ ] baseline 模型（仅临床，仅 omics）对比
* [ ] 模型稳健性验证（噪声, 缺模态）
* [ ] 可解释性分析（gene/protein 富集、文本片段）
* [ ] 法律/伦理/隐私审查（PHI、合规）
* [ ] 完整的训练可复现脚本与容器化部署方案



如果你愿意，我可以：

* 基于你现有数据的**更具体建议**（例如：如果你把基因组给我描述为“VCF with 1000 candidate SNPs” 或者“基因表达矩阵 20k genes × N samples”，我会根据形态给出更精确的 encoder 设计与超参数）；
* 或者把上面的 PyTorch 骨架 **改成支持 cross-attention 的版本**；
* 也可以帮你写数据加载器示例（从 VCF、表格或质谱输出转换为模型可用形式），并演示如何做分层 split、class weight 计算和模型评估脚本。

你想先要哪一部分的具体实现？（我会直接给出代码/步骤，不再问别的问题。）
