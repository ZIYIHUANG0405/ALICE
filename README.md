markdown
# 糖尿病病情进展预测

这是一个独立、可复现的课程作业项目。项目使用线性回归与随机森林回归，对糖尿病患者的基线生理指标与一年后病情进展进行建模，并在一个已执行的 Jupyter Notebook 中完成数据检查、探索性数据分析、训练、评估和结果解释。

> **用途声明：** 本项目仅用于课程学习和机器学习方法演示，不能替代医生判断、临床检查或任何真实医疗诊断。

## 数据集

- 名称：**Diabetes**。
- 来源：通过 `sklearn.datasets.load_diabetes` 从 scikit-learn 内置接口加载。
- 无需用户手动下载数据，也不调用外部 API。
- 共 442 个样本、10 个数值特征，无缺失值。
- 特征为糖尿病患者的基线生理指标：年龄、性别、体质指数（BMI）、平均血压，以及六项血清指标（总胆固醇、低密度脂蛋白、高密度脂蛋白、胆固醇比值、甘油三酯对数、血糖）。
- 目标变量为**一年后病情进展的定量指标**（数值越大表示病情进展越明显）。
- 原始特征已做过标准化处理（均值为 0，范数为 1），本项目在此基础上不再重复缩放特征本身，但建模时仍通过 `Pipeline` 保证流程一致。

## 分析内容

Notebook 从上到下依次完成：

1. 加载数据并检查形状、字段、类型、缺失值和目标变量分布；
2. 绘制目标变量直方图，观察其分布形态；
3. 计算各特征与目标变量的相关系数，筛选最相关的生理指标；
4. 绘制关键特征与目标变量的散点图，观察线性趋势；
5. 以 `test_size=0.2`、`random_state=42` 划分训练集和测试集；
6. 使用带 `StandardScaler` 的 `Pipeline` 训练线性回归；
7. 使用固定随机种子的随机森林回归进行对照；
8. 比较 R²、MAE、RMSE 三项回归指标；
9. 绘制预测值 vs 真实值散点图、残差图和随机森林前 10 个重要特征；
10. 根据实际运行指标自动生成结论，并讨论局限。

所有指标均在测试集上计算。R² 反映模型解释方差的比例，MAE 和 RMSE 反映预测误差的实际大小，三者需结合解读。

## 项目结构
diabetes-progression/
├── .gitignore
├── README.md
├── diabetes.ipynb
└── requirements.txt

text

`.venv/` 只存在于本地且已被 Git 忽略，不会上传到仓库。

## 运行环境

- 开发与验收使用：Python 3.11+
- 主要依赖：NumPy、pandas、Matplotlib、seaborn、scikit-learn、ipykernel、nbconvert
- 每个直接依赖的准确版本记录在 `requirements.txt` 中。

## 创建环境与安装依赖

在项目根目录打开终端，先创建虚拟环境：

```bash
python3 -m venv .venv
macOS / Linux 激活方式：

bash
source .venv/bin/activate
Windows PowerShell 激活方式：

powershell
.venv\Scripts\Activate.ps1
激活后安装锁定版本的直接依赖：

bash
python -m pip install -r requirements.txt
所有依赖必须安装到本项目 .venv，不要使用全局安装或 sudo pip。

在 VS Code 中运行
安装并启用扩展 ms-python.python 与 ms-toolsai.jupyter。

使用 VS Code 打开项目文件夹，再打开 diabetes.ipynb。

点击 Notebook 右上角的内核选择器，选择项目目录中的 .venv Python 解释器。

选择“Run All（全部运行）”，从第一个单元格顺序执行到最后一个单元格。

若 .venv 未立即出现，可执行命令面板中的 “Python: Select Interpreter”，选择 .venv/bin/python；Windows 对应 .venv\Scripts\python.exe。

也可以在已激活环境的终端中重新执行并保存全部输出：

bash
python -m jupyter nbconvert \
  --to notebook \
  --execute \
  --inplace \
  --ExecutePreprocessor.timeout=300 \
  diabetes.ipynb
主要运行结果
Notebook 已从头执行并保存输出。基于固定划分（测试集 89 个样本），本次实际结果为：

模型	R²	MAE	RMSE
Linear Regression	0.4526	42.79	53.85
Random Forest	0.4236	43.85	55.25
在本次固定测试集上，线性回归的 R² 略高于随机森林，MAE 和 RMSE 也略低，说明该数据集的目标变量与特征之间存在较强的线性关系，树模型的非线性拟合优势不明显。实际数值以已执行 Notebook 的指标表为准；不同 Python 或底层数值库环境中，末位小数可能存在轻微差异。

特征重要性显示，bmi、s5（甘油三酯对数）和 bp（平均血压）对病情进展预测贡献较大，提示代谢与血压指标是糖尿病进展的重要相关因素。

可复现性
数据来自 scikit-learn 内置接口，不依赖本地数据路径或手工下载。

数据划分、线性回归和随机森林均固定 random_state=42。

线性回归的标准化仅在训练折内通过 Pipeline 拟合，测试集不参与标准化参数估计。

requirements.txt 锁定本次实际使用的全部直接依赖版本。

Notebook 代码单元可从上到下依次执行。

局限
数据集规模较小（442 个样本），且来源、采集人群与真实临床环境可能不同。

本项目只做一次固定的训练/测试划分，没有外部验证、交叉验证调参或概率校准。

特征来自基线测量值，并不等同于完整病史或生活方式信息。

特征重要性表达模型关联，不代表因果关系或医学机制。

R² 约 0.45 说明仍有大量方差未被解释，模型仅具演示价值。

课程演示结果不能用于个人健康决策或临床诊断。

Git 版本
完整本地版本标记为带说明标签 v1.0.0（First complete release）。克隆仓库后可使用 git checkout v1.0.0 查看本次课程作业的首个完整发布版本。