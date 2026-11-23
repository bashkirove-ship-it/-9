import json
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# 1. Загрузка данных из файла JSON в датафрейм
with open("events.json", "r", encoding="utf-8") as f:
    raw = json.load(f)

# Важный нюанс: в файле данные лежат не списком, а под ключом "events"
df = pd.DataFrame(raw["events"])
df["timestamp"] = pd.to_datetime(df["timestamp"])

# 2. Анализ распределения событий по типам (поле signature)
print("Первые строки датафрейма:")
print(df.head())

print(f"\nВсего событий: {len(df)}")
print(f"Уникальных типов (signature): {df['signature'].nunique()}")
print("\nРаспределение событий по типам:")
print(df["signature"].value_counts())

# 3. Визуализация распределения типов событий
counts = df["signature"].value_counts()

sns.set(style="whitegrid")
plt.figure(figsize=(12, 6))
ax = sns.barplot(x=counts.index, y=counts.values, hue=counts.index,
                 palette="viridis", legend=False)
ax.set_title("Распределение событий ИБ по типам (signature)", fontsize=14)
ax.set_xlabel("Тип события (signature)")
ax.set_ylabel("Количество событий")
plt.xticks(rotation=45, ha="right")
plt.tight_layout()
plt.show()
