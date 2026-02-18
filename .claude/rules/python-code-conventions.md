---
paths:
  - "**/*.py"
  - "Figures/**/*.py"
  - "scripts/**/*.py"
---

# Python 代码规范

**标准:** 高级数据工程师 + 博士研究员质量

---

## 1. 可重现性

- 在文件顶部使用 `random.seed()` 或 `np.random.seed()` 一次（YYYYMMDD 格式）
- 所有包在顶部通过 `import` 语句加载（第三方库后于标准库）
- 所有路径相对于仓库根目录
- 使用 `pathlib.Path()` 和 `.mkdir(parents=True, exist_ok=True)` 创建输出目录
- 使用虚拟环境和 `requirements.txt` 管理依赖

```python
import os
import sys
from pathlib import Path
import random
import numpy as np
import pandas as pd

# 设置随机种子
random.seed(20240115)
np.random.seed(20240115)

# 相对路径
BASE_DIR = Path(__file__).resolve().parent.parent
OUTPUT_DIR = BASE_DIR / "outputs"
OUTPUT_DIR.mkdir(parents=True, exist_ok=True)
```

## 2. PEP8 合规性

- 行长度 <= 100 字符（见第 10 节的例外）
- 函数和变量使用 `snake_case` 命名
- 类使用 `PascalCase` 命名
- 常量使用 `UPPER_CASE` 命名
- 使用 4 个空格进行缩进（不使用制表符）
- 在导入之间、类之间、函数之间添加空行

```python
# 正确的导入组织
import os
import sys
from pathlib import Path

import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

from local_module import helper_function
```

## 3. 类型提示（Python 3.7+）

- 为所有函数参数和返回值添加类型提示
- 使用 `from typing import` 导入复杂类型
- 使用 `Optional` 表示可为 None 的值

```python
from typing import Dict, List, Tuple, Optional
import pandas as pd
import numpy as np

def calculate_statistics(
    data: pd.DataFrame,
    columns: List[str],
    output_path: Optional[str] = None
) -> Dict[str, np.ndarray]:
    """计算统计量。
    
    Args:
        data: 输入数据框
        columns: 要分析的列名
        output_path: 可选的输出路径
        
    Returns:
        包含统计量的字典
    """
    results = {}
    for col in columns:
        results[col] = {
            "mean": data[col].mean(),
            "std": data[col].std()
        }
    return results
```

## 4. 文档字符串标准

使用 NumPy 或 Google 风格的文档字符串。以下示例使用 NumPy 风格：

```python
def regression_analysis(
    X: np.ndarray,
    y: np.ndarray,
    method: str = "ols"
) -> Dict[str, float]:
    """执行回归分析。
    
    进行详细的描述，解释函数的目的和使用场景。
    
    Parameters
    ----------
    X : np.ndarray
        形状为 (n_samples, n_features) 的特征矩阵
    y : np.ndarray
        形状为 (n_samples,) 的目标向量
    method : str, optional
        回归方法，默认为 'ols'（最小二乘法）
        
    Returns
    -------
    dict
        包含系数和 R² 的字典
        
    Raises
    ------
    ValueError
        如果数据维度不匹配
        
    Notes
    -----
    该函数实现了标准的 OLS 回归。
    详见文献：[参考文献]
    
    Examples
    --------
    >>> X = np.random.randn(100, 5)
    >>> y = X @ np.array([1, -0.5, 0.2, 0.1, -0.3]) + np.random.randn(100) * 0.1
    >>> result = regression_analysis(X, y)
    >>> print(result["r_squared"])
    """
    pass
```

## 5. 导入组织

遵循以下导入顺序（用空行分隔）：

```python
# 1. 标准库
import os
import sys
import pickle
import json
from pathlib import Path
from typing import Dict, List, Optional

# 2. 第三方库
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import nano_banana as nb

# 3. 本地模块
from . import local_module
from .utils import helper_function
```

## 6. 虚拟环境和依赖管理

```bash
# 创建虚拟环境
python -m venv venv

# 激活虚拟环境（Linux/Mac）
source venv/bin/activate

# 激活虚拟环境（Windows）
venv\Scripts\activate

# 安装依赖
pip install -r requirements.txt

# 生成 requirements.txt
pip freeze > requirements.txt
```

**requirements.txt 模板：**
```
numpy>=1.21.0
pandas>=1.3.0
matplotlib>=3.4.0
seaborn>=0.11.0
nano-banana>=0.1.0
scipy>=1.7.0
scikit-learn>=1.0.0
jupyter>=1.0.0
```

## 7. 函数设计

- `snake_case` 命名，动词-名词模式
- 完整的文档字符串（NumPy 或 Google 风格）
- 默认参数，避免魔数
- 返回结构化对象（字典、DataFrame 或命名元组）
- 限制函数长度（理想情况 < 50 行）

```python
def estimate_regression_coefficients(
    X: np.ndarray,
    y: np.ndarray,
    weights: Optional[np.ndarray] = None
) -> Dict[str, np.ndarray]:
    """使用加权最小二乘法估计回归系数。
    
    Parameters
    ----------
    X : np.ndarray
        特征矩阵，形状为 (n_samples, n_features)
    y : np.ndarray
        目标向量，形状为 (n_samples,)
    weights : np.ndarray, optional
        样本权重，如果为 None，使用均匀权重
        
    Returns
    -------
    dict
        包含 'coefficients'、'fitted_values' 和 'residuals' 的字典
    """
    if weights is None:
        weights = np.ones_like(y)
    
    # 处理权重
    W_sqrt = np.diag(np.sqrt(weights))
    X_weighted = W_sqrt @ X
    y_weighted = W_sqrt @ y
    
    # 估计系数
    coefficients = np.linalg.lstsq(X_weighted, y_weighted, rcond=None)[0]
    fitted_values = X @ coefficients
    residuals = y - fitted_values
    
    return {
        "coefficients": coefficients,
        "fitted_values": fitted_values,
        "residuals": residuals
    }
```

## 8. 异常处理最佳实践

```python
from pathlib import Path
import logging

# 配置日志
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

def load_data(filepath: str) -> pd.DataFrame:
    """加载数据文件。
    
    Parameters
    ----------
    filepath : str
        数据文件路径
        
    Returns
    -------
    pd.DataFrame
        加载的数据
        
    Raises
    ------
    FileNotFoundError
        如果文件不存在
    ValueError
        如果文件格式无效
    """
    try:
        data = pd.read_csv(filepath)
        logger.info(f"成功加载数据：{filepath}，形状：{data.shape}")
        return data
    except FileNotFoundError:
        logger.error(f"文件未找到：{filepath}")
        raise
    except Exception as e:
        logger.error(f"加载数据时出错：{str(e)}")
        raise ValueError(f"无法加载文件 {filepath}") from e
```

## 9. Pandas 学术数据最佳实践

```python
import pandas as pd
import numpy as np

# 优先使用方法链
result = (
    data
    .query("year >= 2020")
    .groupby(["region", "sector"])
    .agg({
        "revenue": ["mean", "std"],
        "employees": "sum"
    })
    .reset_index()
    .rename(columns={"revenue": "revenue_stats"})
)

# 避免链式赋值
df.loc[mask, "col"] = value  # 好的做法

# 使用 .copy() 避免 SettingWithCopyWarning
df_subset = df[df["year"] == 2020].copy()

# 明确数据类型
df["date"] = pd.to_datetime(df["date"])
df["category"] = df["category"].astype("category")

# 处理缺失值
df_clean = df.dropna(subset=["key_column"])
df_filled = df.fillna(method="forward")  # 也可以指定其他填充方式
```

## 10. 可视化和 NanoBanana 支持

### matplotlib 和 seaborn 基础

```python
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np

# 机构色板
PRIMARY_BLUE = "#012169"
PRIMARY_GOLD = "#f2a900"
ACCENT_GRAY = "#525252"
POSITIVE_GREEN = "#15803d"
NEGATIVE_RED = "#b91c1c"

# 设置风格
sns.set_style("whitegrid")
plt.rcParams["figure.facecolor"] = "white"
plt.rcParams["axes.facecolor"] = "white"

# 自定义主题函数
def apply_custom_theme(ax=None, title_color=PRIMARY_BLUE):
    """应用自定义主题到 Matplotlib 轴。"""
    if ax is None:
        ax = plt.gca()
    
    ax.spines["top"].set_visible(False)
    ax.spines["right"].set_visible(False)
    ax.set_title(ax.get_title(), color=title_color, fontweight="bold", fontsize=12)
    return ax

# Beamer 图形尺寸
BEAMER_WIDTH = 12
BEAMER_HEIGHT = 5
```

### NanoBanana 综合支持

**何时使用 NanoBanana：**

NanoBanana 是一个专为学术论文和演示设计的绘图库，特别适合以下场景：

- 回归诊断图和系数图
- 边际效应（Marginal Effects）可视化
- 预测间隔和置信区间
- 复杂的多面板学术图表
- 需要发布质量输出的统计可视化

**何时使用 matplotlib/seaborn：**

- 快速探索性数据分析
- 简单的分布图、散点图、直方图
- 需要高度自定义的图形
- 实时交互式绘图

**安装：**
```bash
pip install nano-banana
```

**导入：**
```python
import nano_banana as nb
# 或
from nano_banana import nb
```

**NanoBanana 最佳实践和示例：**

```python
import nano_banana as nb
import pandas as pd
import numpy as np

# 示例 1：回归系数图
def plot_coefficients(coefficients: pd.DataFrame, filename: str):
    """绘制回归系数及其置信区间。
    
    Parameters
    ----------
    coefficients : pd.DataFrame
        包含列：'variable', 'estimate', 'ci_lower', 'ci_upper'
    filename : str
        输出文件名
    """
    fig = nb.coefplot(
        coefficients["estimate"],
        ci_lower=coefficients["ci_lower"],
        ci_upper=coefficients["ci_upper"],
        labels=coefficients["variable"],
        title="Regression Coefficients with 95% CI"
    )
    fig.savefig(filename, dpi=300, bbox_inches="tight", facecolor="white")
    plt.close()

# 示例 2：边际效应图
def plot_marginal_effects(x_range: np.ndarray, effects: np.ndarray, 
                         se: np.ndarray, filename: str):
    """绘制连续变量的边际效应。
    
    Parameters
    ----------
    x_range : np.ndarray
        变量范围
    effects : np.ndarray
        边际效应点估计
    se : np.ndarray
        标准误差
    filename : str
        输出文件名
    """
    # 计算置信区间（95%）
    ci_lower = effects - 1.96 * se
    ci_upper = effects + 1.96 * se
    
    fig, ax = plt.subplots(figsize=(BEAMER_WIDTH, BEAMER_HEIGHT))
    
    ax.plot(x_range, effects, color=PRIMARY_BLUE, linewidth=2, label="Point Estimate")
    ax.fill_between(x_range, ci_lower, ci_upper, alpha=0.3, color=PRIMARY_BLUE)
    
    ax.axhline(y=0, color=ACCENT_GRAY, linestyle="--", alpha=0.5)
    ax.set_xlabel("Variable")
    ax.set_ylabel("Marginal Effect")
    ax.set_title("Marginal Effects Plot")
    ax.legend()
    apply_custom_theme(ax)
    
    plt.savefig(filename, dpi=300, bbox_inches="tight", facecolor="white")
    plt.close()

# 示例 3：预测区间图
def plot_predictions(x: np.ndarray, y_pred: np.ndarray, 
                    pred_lower: np.ndarray, pred_upper: np.ndarray,
                    x_data: np.ndarray, y_data: np.ndarray,
                    filename: str):
    """绘制预测值及其区间。
    
    Parameters
    ----------
    x : np.ndarray
        预测范围
    y_pred : np.ndarray
        点预测
    pred_lower : np.ndarray
        预测下界
    pred_upper : np.ndarray
        预测上界
    x_data, y_data : np.ndarray
        原始数据
    filename : str
        输出文件名
    """
    fig, ax = plt.subplots(figsize=(BEAMER_WIDTH, BEAMER_HEIGHT))
    
    # 绘制原始数据
    ax.scatter(x_data, y_data, alpha=0.5, s=30, color=ACCENT_GRAY, label="Data")
    
    # 绘制预测和区间
    ax.plot(x, y_pred, color=PRIMARY_BLUE, linewidth=2, label="Prediction")
    ax.fill_between(x, pred_lower, pred_upper, alpha=0.3, color=PRIMARY_BLUE,
                    label="95% Prediction Interval")
    
    ax.set_xlabel("X")
    ax.set_ylabel("Y")
    ax.set_title("Regression Fit with Prediction Intervals")
    ax.legend()
    apply_custom_theme(ax)
    
    plt.savefig(filename, dpi=300, bbox_inches="tight", facecolor="white")
    plt.close()

# 示例 4：回归诊断
def plot_regression_diagnostics(residuals: np.ndarray, fitted: np.ndarray,
                               filename_prefix: str):
    """绘制四个标准回归诊断图。
    
    Parameters
    ----------
    residuals : np.ndarray
        模型残差
    fitted : np.ndarray
        拟合值
    filename_prefix : str
        输出文件名前缀
    """
    fig, axes = plt.subplots(2, 2, figsize=(12, 10))
    
    # 1. 残差 vs 拟合值
    axes[0, 0].scatter(fitted, residuals, alpha=0.5, color=PRIMARY_BLUE)
    axes[0, 0].axhline(y=0, color=NEGATIVE_RED, linestyle="--")
    axes[0, 0].set_xlabel("Fitted Values")
    axes[0, 0].set_ylabel("Residuals")
    axes[0, 0].set_title("Residuals vs Fitted")
    apply_custom_theme(axes[0, 0])
    
    # 2. Q-Q 图
    from scipy import stats
    stats.probplot(residuals, dist="norm", plot=axes[0, 1])
    axes[0, 1].set_title("Q-Q Plot")
    apply_custom_theme(axes[0, 1])
    
    # 3. 标准化残差的平方根
    std_residuals = np.sqrt(np.abs(residuals / np.std(residuals)))
    axes[1, 0].scatter(fitted, std_residuals, alpha=0.5, color=PRIMARY_BLUE)
    axes[1, 0].set_xlabel("Fitted Values")
    axes[1, 0].set_ylabel("√|Standardized Residuals|")
    axes[1, 0].set_title("Scale-Location")
    apply_custom_theme(axes[1, 0])
    
    # 4. 残差分布
    axes[1, 1].hist(residuals, bins=20, color=PRIMARY_BLUE, alpha=0.7, edgecolor="black")
    axes[1, 1].set_xlabel("Residuals")
    axes[1, 1].set_ylabel("Frequency")
    axes[1, 1].set_title("Residual Distribution")
    apply_custom_theme(axes[1, 1])
    
    plt.tight_layout()
    plt.savefig(f"{filename_prefix}_diagnostics.png", dpi=300, bbox_inches="tight", 
               facecolor="white")
    plt.close()
```

### 图形尺寸和导出模式

```python
import matplotlib.pyplot as plt

# Beamer 演示尺寸
BEAMER_WIDTH = 12
BEAMER_HEIGHT = 5

# 学术论文尺寸（单列）
PAPER_SINGLE_COL_WIDTH = 3.5
PAPER_SINGLE_COL_HEIGHT = 2.5

# 学术论文尺寸（双列）
PAPER_DOUBLE_COL_WIDTH = 7.0
PAPER_DOUBLE_COL_HEIGHT = 2.5

# 导出函数
def save_figure(fig: plt.Figure, filepath: str, dpi: int = 300,
               transparent: bool = True, bbox_inches: str = "tight"):
    """保存图形为高质量文件。
    
    Parameters
    ----------
    fig : plt.Figure
        要保存的图形对象
    filepath : str
        输出文件路径
    dpi : int
        分辨率（每英寸点数）
    transparent : bool
        是否使用透明背景
    bbox_inches : str
        边框设置
    """
    fig.savefig(
        filepath,
        dpi=dpi,
        transparent=transparent,
        bbox_inches=bbox_inches,
        facecolor="white" if not transparent else None
    )
    plt.close(fig)

# 使用示例
fig, ax = plt.subplots(figsize=(BEAMER_WIDTH, BEAMER_HEIGHT))
ax.plot([1, 2, 3], [1, 4, 9])
save_figure(fig, "output/plot.png")
```

## 11. Pickle 数据模式

**重计算保存为 pickle；后续分析加载预计算数据。**

```python
import pickle
from pathlib import Path

# 保存计算结果
def save_results(results: dict, filename: str, output_dir: str = "outputs"):
    """保存计算结果为 pickle 文件。"""
    output_path = Path(output_dir)
    output_path.mkdir(parents=True, exist_ok=True)
    filepath = output_path / f"{filename}.pkl"
    
    with open(filepath, "wb") as f:
        pickle.dump(results, f)
    print(f"Results saved to {filepath}")

# 加载计算结果
def load_results(filename: str, input_dir: str = "outputs") -> dict:
    """从 pickle 文件加载计算结果。"""
    filepath = Path(input_dir) / f"{filename}.pkl"
    
    with open(filepath, "rb") as f:
        results = pickle.load(f)
    return results

# 使用示例
results = {
    "coefficients": np.array([1.2, -0.5, 0.3]),
    "r_squared": 0.85,
    "residuals": np.array([...])
}
save_results(results, "regression_model")

# 后续加载
loaded = load_results("regression_model")
```

## 12. 常见陷阱

| 陷阱 | 影响 | 预防 |
|------|------|------|
| 未设置 `random.seed()` | 不可重现的结果 | 在脚本顶部设置一次种子 |
| 硬编码路径 | 在其他机器上中断 | 使用相对路径和 `pathlib.Path` |
| 修改传入的可变对象 | 副作用和错误 | 在修改前使用 `.copy()` |
| 缺少类型提示 | 代码理解困难 | 为所有函数添加类型提示 |
| 缺少异常处理 | 意外崩溃 | 使用 try-except 并记录错误 |
| Pandas 链式赋值警告 | 不稳定的代码 | 使用 `.loc[]` 或 `.copy()` |
| 图形背景不透明 | 幻灯片上出现白框 | 始终使用 `facecolor="white"` 或 `transparent=True` |
| 导入未列在 requirements.txt | 依赖项丢失 | 定期运行 `pip freeze > requirements.txt` |

## 13. 行长度和数学例外

**标准:** 保持行长 <= 100 字符。

**例外：数学公式** -- 如果满足以下条件，行可以超过 100 字符：

1. 换行会破坏数学的可读性（影响函数、矩阵运算、有限差分近似、与论文公式匹配的公式实现）
2. 内联注释解释数学操作：
   ```python
   # 筛选投影：残差与基函数 P_k 的内积
   alpha_k = np.sum(r_i * basis[:, k]) / np.sum(basis[:, k]**2)
   ```
3. 该行位于数值密集部分（模拟循环、估计程序、推断计算）

**质量关卡影响:**
- 非数学代码中的长行：轻微扣分（每行 -1 到 -2）
- 文档化数学部分中的长行：无扣分

## 14. 代码质量检查清单

```
[ ] 导入在顶部组织（标准库 → 第三方 → 本地）
[ ] random.seed() 在顶部设置一次（YYYYMMDD 格式）
[ ] 所有路径相对于仓库根目录
[ ] 函数使用完整的文档字符串（NumPy 或 Google 风格）
[ ] 函数和参数有类型提示
[ ] 使用虚拟环境和 requirements.txt
[ ] 图形：透明或白色背景，明确的尺寸
[ ] 重计算结果：保存为 pickle
[ ] 遵循 PEP8（行长 <= 100，例外已文档化）
[ ] 异常处理和日志记录
[ ] 注释解释 WHY 而非 WHAT
[ ] 没有硬编码的路径或魔数
[ ] Pandas 操作避免 SettingWithCopyWarning
[ ] 代码在虚拟环境中测试过
```
