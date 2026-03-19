# syntecxhub_Statistical_Plots-Distribution_Analysis
This project analyzes data distributions using histograms, KDE plots, and boxplots. It compares groups to study variability, detects outliers using the IQR method, and measures skewness. The results are visualized and saved as plots, with a brief interpretation to understand data patterns and differences effectively.
import numpy as np
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

np.random.seed(42)

data = pd.DataFrame({
    "Group": ["A"]*100 + ["B"]*100,
    "Values": np.concatenate([
        np.random.normal(50, 10, 100),
        np.random.normal(60, 15, 100)
    ])
})

plt.figure()
sns.histplot(data=data, x="Values", hue="Group", kde=True)
plt.title("Histogram with KDE")
plt.savefig("hist_kde.png")
plt.show()

plt.figure()
sns.boxplot(data=data, x="Group", y="Values")
plt.title("Boxplot for Outlier Detection")
plt.savefig("boxplot.png")
plt.show()

print("\nStatistical Summary:")
print(data.groupby("Group")["Values"].describe())

print("\nSkewness:")
print(data.groupby("Group")["Values"].skew())

def detect_outliers(df):
    Q1 = df["Values"].quantile(0.25)
    Q3 = df["Values"].quantile(0.75)
    IQR = Q3 - Q1
    lower = Q1 - 1.5 * IQR
    upper = Q3 + 1.5 * IQR
    outliers = df[(df["Values"] < lower) | (df["Values"] > upper)]
    return outliers

print("\nOutliers in Group A:")
print(detect_outliers(data[data["Group"]=="A"]))

print("\nOutliers in Group B:")
print(detect_outliers(data[data["Group"]=="B"]))
