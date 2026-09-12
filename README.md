# Data Exploration with Python

## ภาพรวม (Overview)

โปรเจกต์นี้เป็น **Data Exploration Notebook** ที่ใช้สำหรับสำรวจและวิเคราะห์ข้อมูลตัวอย่าง (Sample Data) ของลูกค้า โดยเน้นการสร้างภาพข้อมูล (Data Visualization) ในรูปแบบต่างๆ เพื่อทำความเข้าใจการกระจายตัวของข้อมูล ความสัมพันธ์ระหว่างตัวแปร และรูปแบบ (Patterns) ที่ซ่อนอยู่ในข้อมูล

โปรเจกต์นี้เหมาะสำหรับ:
- ผู้เริ่มต้นเรียนรู้ Data Visualization ด้วย Python
- การศึกษาหลักการสร้างกราฟประเภทต่างๆ
- การทำความเข้าใจข้อมูลก่อนนำไปวิเคราะห์ต่อหรือสร้างโมเดล Machine Learning

---

## สิ่งที่ต้องใช้ (Requirements)

| รายการ | เวอร์ชัน |
|--------|----------|
| Python | 3.12+ |
| NumPy | 2.5+ |
| Pandas | 3.0+ |
| Matplotlib | 3.11+ |
| Plotly | 7.0+ |
| Statsmodels | 0.15+ |

---

## การติดตั้งและใช้งาน (Setup & Usage)

### 1. สร้าง Virtual Environment

```bash
python3 -m venv .venv
```

### 2. เปิดใช้งาน Virtual Environment

```bash
# macOS / Linux
source .venv/bin/activate

# Windows
.venv\Scripts\activate
```

### 3. ติดตั้ง Packages ที่จำเป็น

```bash
pip install numpy pandas matplotlib plotly statsmodels
```

### 4. เปิด Jupyter Notebook

```bash
jupyter notebook notebook_01.ipynb
```

### 5. รัน Cells ทีละ Cell หรือทั้งหมด

กด `Shift + Enter` ในแต่ละ Cell เพื่อรัน หรือเลือก `Run All` จากเมนู

---

## โครงสร้างข้อมูล (Dataset Structure)

ข้อมูลตัวอย่างประกอบด้วยลูกค้า 15 คน โดยมีคอลัมน์ดังนี้:

| คอลัมน์ | ประเภท | คำอธิบาย | ตัวอย่างค่า |
|---------|--------|----------|-------------|
| `Cust_ID` | int | รหัสลูกค้า | 1, 2, 3, ... |
| `Age` | int | อายุ (ปี) | 25, 32, 40, ... |
| `Income` | int | รายได้ (บาท) | 25000, 32000, 45000, ... |
| `Spend_SCR` | int | คะแนนการใช้จ่าย (0-100) | 60, 70, 55, ... |
| `Gender` | str | เพศ | M (ชาย), F (หญิง) |
| `Join_Date` | datetime | วันที่สมัครสมาชิก | 2020-01-31, ... |
| `Is_Member` | bool | สถานะสมาชิก | True, False |

### ตัวอย่างข้อมูล

```
   Cust_ID  Age  Income  Spend_SCR Gender  Join_Date  Is_Member
0        1   25   25000         60      F 2020-01-31       True
1        2   32   32000         70      M 2020-02-29       True
2        3   40   45000         55      M 2020-03-31      False
...
```

---

## รายการ Visualization (12 รูปแบบ)

### 1. 📊 Histogram — การกระจายตัวของรายได้ (Income Distribution)

**วัตถุประสงค์:** ดูการกระจายของข้อมูลรายได้ว่า集中在ช่วงใด มีค่าเฉลี่ย (Mean), กลาง (Median), และฐานนิยม (Mode) เท่าใด

**Concepts สำคัญ:**
- **Histogram** แสดงความถี่ของข้อมูลโดยแบ่งเป็น "bins" (ช่วงค่า)
- **Mean** (เส้นสีแดง) = ค่าเฉลี่ยเลขคณิต
- **Median** (เส้นสีเขียว) = ค่ากลางที่แบ่งข้อมูลครึ่งหนึ่ง
- **Mode** (เส้นสีส้ม) = ค่าที่ปรากฏบ่อยที่สุด

**พารามิเตอร์ที่ใช้:**
```python
plt.hist(df["Income"], bins=8, color="skyblue", edgecolor="black", alpha=0.8)
```
- `bins=8` : แบ่งข้อมูลเป็น 8 ช่วง
- `alpha=0.8` : ความโปร่งใสของกราฟ (0-1)
- `edgecolor="black"` : เส้นขอบของแต่ละแท่ง

**สิ่งที่เรียนรู้:**
- เปรียบเทียบ Mean, Median, Mode ได้ว่าใกล้เคียงกันหรือไม่
- หาก Mean > Median แสดงว่าข้อมูลเบไปทางขวา (Right-skewed)
- หาก Mean < Median แสดงว่าข้อมูลเบไปทางซ้าย (Left-skewed)

---

### 2. 📦 Box Plot — สรุปสถิติของรายได้

**วัตถุประสงค์:** แสดงสรุปสถิติ 5 ค่า (Five-Number Summary) ของข้อมูลรายได้

**Concepts สำคัญ:**
- **Minimum** : ค่าต่ำสุดที่ไม่ถือเป็น Outlier
- **Q1 (25%)** : ควอไทล์ที่ 1 — 25% ของข้อมูลมีค่าน้อยกว่านี้
- **Median (Q2, 50%)** : ค่ากลางของข้อมูล
- **Q3 (75%)** : ควอไทล์ที่ 3 — 75% ของข้อมูลมีค่าน้อยกว่านี้
- **Maximum** : ค่าสูงสุดที่ไม่ถือเป็น Outlier
- **IQR (Interquartile Range)** = Q3 - Q1
- **Outlier** : จุดข้อมูลที่อยู่นอกเหนือจาก `1.5 × IQR`

**พารามิเตอร์ที่ใช้:**
```python
plt.boxplot(df[feature], vert=True, patch_artist=True,
            boxprops=dict(facecolor="skyblue", color="black"),
            medianprops=dict(color="red", linewidth=2))
```

**สิ่งที่เรียนรู้:**
- ระบุค่าผิดปกติ (Outliers) ได้จากจุดที่อยู่นอก Whiskers
- ดูความสมมาตรของข้อมูลจากตำแหน่งของ Median ใน Box
- ดูความยาวของ Box (IQR) ว่าข้อมูลกระจายตัวมากน้อยเพียงใด

---

### 3. 📊 Bar Chart — ความถี่และสัดส่วนของเพศ

**วัตถุประสงค์:** แสดงจำนวน (Frequency) และสัดส่วน (Proportion) ของแต่ละเพศ

**Concepts สำคัญ:**
- **Frequency (ความถี่)** : จำนวนของแต่ละหมวดหมู่
- **Proportion (สัดส่วน)** : สัดส่วนของแต่ละหมวดหมู่เมื่อเทียบกับทั้งหมด (รวมกันได้ 1.0 หรือ 100%)

**พารามิเตอร์ที่ใช้:**
```python
freq = df['Gender'].value_counts()           # นับจำนวน
prop = df['Gender'].value_counts(normalize=True)  # คำนวณสัดส่วน
```

**สิ่งที่เรียนรู้:**
- เปรียบเทียบจำนวนระหว่างเพศชายและเพศหญิงได้ทันที
- สัดส่วนช่วยให้เข้าใจว่าแต่ละกลุ่มคิดเป็นกี่เปอร์เซ็นต์ของทั้งหมด
- เหมาะกับข้อมูลเชิงหมวดหมู่ (Categorical Data)

---

### 4. 🔵 Scatter Plot — ความสัมพันธ์ระหว่างอายุและรายได้ (พื้นฐาน)

**วัตถุประสงค์:** ดูความสัมพันธ์ระหว่างตัวแปรต่อเนื่อง 2 ตัว คือ อายุ (Age) และ รายได้ (Income)

**Concepts สำคัญ:**
- **Scatter Plot** แสดงจุดข้อมูลแต่ละจุดบนแกน X และ Y
- หากจุดเรียงเป็นเส้นจากซ้ายล่างไปขวาบน = ความสัมพันธ์เชิงบวก (Positive Correlation)
- หากจุดเรียงเป็นเส้นจากซ้ายบนไปขวาล่าง = ความสัมพันธ์เชิงลบ (Negative Correlation)
- หากจุดกระจายไม่มีรูปแบบ = ไม่มีความสัมพันธ์

**พารามิเตอร์ที่ใช้:**
```python
plt.scatter(df["Age"], df["Income"], c="blue", alpha=0.7, edgecolors="black")
```

**สิ่งที่เรียนรู้:**
- อายุกับรายได้มีความสัมพันธ์เชิงบวกหรือไม่
- ระบุค่าผิดปกติ (Outliers) ที่ไม่อยู่ในรูปแบบทั่วไป

---

### 5. 🎨 Scatter Plot — เพิ่มมิติสีและขนาด (Gender + Spend_SCR)

**วัตถุประสงค์:** เพิ่มข้อมูลมิติที่ 3 และ 4 ลงใน Scatter Plot โดยใช้ **สี** และ **ขนาด** ของจุด

**Concepts สำคัญ:**
- **Color (สี)** : แสดงมิติที่ 3 (Gender: สีน้ำเงิน = ชาย, สีชมพู = หญิง)
- **Size (ขนาด)** : แสดงมิติที่ 4 (Spend_SCR: คะแนนการใช้จ่าย)

**พารามิเตอร์ที่ใช้:**
```python
colors = df["Gender"].map({"M": "blue", "F": "pink"})
sizes = df["Spend_SCR"] * 5
plt.scatter(df["Age"], df["Income"], c=colors, s=sizes, alpha=0.7, edgecolors="black")
```

**สิ่งที่เรียนรู้:**
- สามารถแสดงข้อมูลได้สูงสุด 5 มิติในกราฟ 2D (X, Y, Color, Size, Shape)
- ผู้ดูสามารถเห็นรูปแบบตามเพศได้ชัดเจน (จุดสีน้ำเงิน vs สีชมพู)
- จุดที่ใหญ่กว่าหมายถึงการใช้จ่ายสูงกว่า

---

### 6. 🌐 3D Scatter Plot — เพิ่มมิติความลึก (Matplotlib)

**วัตถุประสงค์:** แสดงข้อมูล 3 ตัวแปรพร้อมกันในกราฟ 3 มิติ

**Concepts สำคัญ:**
- แกน X = Age, แกน Y = Income, แกน Z = Spend_SCR
- สามารถหมุนกราฟเพื่อดูจากมุมมองต่างๆ ได้

**พารามิเตอร์ที่ใช้:**
```python
from mpl_toolkits.mplot3d import Axes3D
ax = fig.add_subplot(111, projection="3d")
ax.scatter(df["Age"], df["Income"], df["Spend_SCR"], c=colors, s=60, alpha=0.8)
```

**สิ่งที่เรียนรู้:**
- ความสัมพันธ์ระหว่าง Age, Income, และ Spend_SCR ใน 3 มิติ
- เหมาะกับข้อมูลที่มีตัวแปรต่อเนื่อง 3 ตัว

---

### 7. 🌐 Interactive 3D Scatter Plot — Plotly

**วัตถุประสงค์:** สร้างกราฟ 3 มิติแบบ **โต้ตอบได้** (Interactive) สามารถซูม หมุน เปลี่ยนมุมมองได้

**Concepts สำคัญ:**
- **Plotly** เป็น Library สำหรับสร้าง Interactive Visualization
- สามารถ Zoom In/Zoom Out, หมุนกราฟ, และ Hover ดูค่าได้
- ใช้ `px.scatter_3d()` สำหรับสร้าง 3D Scatter Plot

**พารามิเตอร์ที่ใช้:**
```python
import plotly.express as px
fig = px.scatter_3d(df,
                    x="Age", y="Income", z="Spend_SCR",
                    color="Gender", symbol="Is_Member",
                    size="Spend_SCR",
                    title="Interactive 3D Scatter Plot")
```

**สิ่งที่เรียนรู้:**
- Plotly ให้ประสบการณ์ที่ดีกว่า Matplotlib สำหรับ Interactive Visualization
- `color` = แยกสีตาม Gender
- `symbol` = แยกสัญลักษณ์ตาม Is_Member
- `size` = ขนาดจุดตาม Spend_SCR

---

### 8. 🌡️ Correlation Heatmap — เมทริกซ์ความสัมพันธ์

**วัตถุประสงค์:** แสดงค่าสหสัมพันธ์ (Correlation) ระหว่างตัวแปรต่อเนื่องทั้งหมด

**Concepts สำคัญ:**
- **Correlation Coefficient (r)** มีค่าระหว่าง -1 ถึง +1
  - `r = +1` : สัมสัมพันธ์เชิงบวกสมบูรณ์
  - `r = 0` : ไม่มีสัมพันธ์
  - `r = -1` : สัมสัมพันธ์เชิงลบสมบูรณ์
- **Heatmap** ใช้สีแสดงค่า — สีแดง = สูง, สีน้ำเงิน = ต่ำ

**พารามิเตอร์ที่ใช้:**
```python
corr = df[["Age","Income","Spend_SCR"]].corr()
plt.imshow(corr, cmap="coolwarm", interpolation="nearest")
```

**สิ่งที่เรียนรู้:**
- Age กับ Income อาจมีความสัมพันธ์เชิงบวก (r > 0)
- Spend_SCR อาจมีความสัมพันธ์กับ Income อย่างไร
- ช่วยเลือกตัวแปรสำคัญสำหรับโมเดล Machine Learning

---

### 9. 📊 Pair Plot — กราฟทุกคู่ของตัวแปร

**วัตถุประสงค์:** แสดง Scatter Plot ทุกคู่ของตัวแปร และ Histogram ของแต่ละตัวแปรในกราฟเดียว

**Concepts สำคัญ:**
- **Diagonal (แนวทแยง)** : Histogram ของแต่ละตัวแปร
- **Off-diagonal** : Scatter Plot ของแต่ละคู่ตัวแปร
- เหมาะกับข้อมูลที่มีตัวแปรต่อเนื่องหลายตัว

**พารามิเตอร์ที่ใช้:**
```python
features = ["Age","Income","Spend_SCR"]
for i, f1 in enumerate(features):
    for j, f2 in enumerate(features):
        ax = axes[i,j]
        if i == j:
            ax.hist(df[f1], color="skyblue", edgecolor="black")  # Diagonal
        else:
            ax.scatter(df[f2], df[f1], c=colors, alpha=0.7)      # Off-diagonal
```

**สิ่งที่เรียนรู้:**
- เห็นภาพรวมของข้อมูลทั้งหมดในกราฟเดียว
- ระบุความสัมพันธ์ระหว่างตัวแปรแต่ละคู่ได้รวดเร็ว
- ตรวจสอบการกระจายตัวของแต่ละตัวแปรพร้อมกัน

---

### 10. 🥧 Pie Chart — สัดส่วนของเพศ

**วัตถุประสงค์:** แสดงสัดส่วนของแต่ละเพศเป็นเปอร์เซ็นต์

**Concepts สำคัญ:**
- **Pie Chart** เหมาะกับข้อมูลหมวดหมู่ที่มีไม่เกิน 4-5 หมวด
- แสดงสัดส่วนเป็นเปอร์เซ็นต์อัตโนมัติ

**พารามิเตอร์ที่ใช้:**
```python
count_vals = df["Gender"].value_counts()
plt.pie(count_vals, labels=count_vals.index,
        autopct="%1.1f%%", colors=["skyblue", "salmon"],
        startangle=90)
```

**สิ่งที่เรียนรู้:**
- เพศใดมีสัดส่วนมากกว่า
- ควรใช้ Pie Chart กับข้อมูลที่ไม่ซับซ้อนเกินไป

---

### 11. 📊 Grouped Bar Chart — รายได้เฉลี่ยตามเพศและสถานะสมาชิก

**วัตถุประสงค์:** เปรียบเทียบค่าเฉลี่ยรายได้ระหว่างเพศชายและหญิง โดยแยกตามสถานะสมาชิก

**Concepts สำคัญ:**
- **Grouped Bar Chart** แสดงหลายกลุ่มในแท่งเคียงกัน
- ใช้ `groupby()` เพื่อรวมข้อมูลตามหลายคอลัมน์

**พารามิเตอร์ที่ใช้:**
```python
group_mean = df.groupby(["Gender","Is_Member"])["Income"].mean().unstack()
group_mean.plot(kind="bar", figsize=(8,6), color=["skyblue","salmon"])
```

**สิ่งที่เรียนรู้:**
- สมาชิก (Is_Member=True) มีรายได้เฉลี่ยสูงกว่าหรือไม่
- ชาย vs หญิง มีรายได้เฉลี่ยต่างกันอย่างไร
- ช่วยในการตัดสินใจด้าน Marketing Segmentation

---

### 12. 📊 Stacked Bar Chart — จำนวนตามเพศและสถานะสมาชิก

**วัตถุประสงค์:** แสดงจำนวนลูกค้าโดยแยกชั้นตามสถานะสมาชิก

**Concepts สำคัญ:**
- **Stacked Bar Chart** แสดงส่วนย่อยซ้อนกันในแท่งเดียว
- ความสูงรวมของแท่ง = ค่ารวมทั้งหมด

**พารามิเตอร์ที่ใช้:**
```python
group_count = df.groupby(["Gender","Is_Member"]).size().unstack()
group_count.plot(kind="bar", stacked=True, figsize=(8,6))
```

**สิ่งที่เรียนรู้:**
- สัดส่วนสมาชิก vs ไม่เป็นสมาชิกในแต่ละเพศ
- จำนวนลูกค้ารวมในแต่ละกลุ่ม

---

### 13. 🌡️ Heatmap — ความสัมพันธ์ระหว่างเพศและสถานะสมาชิก

**วัตถุประสงค์:** แสดงจำนวนลูกค้าในแต่ละกลุ่มด้วย Heatmap

**Concepts สำคัญ:**
- ใช้ `pd.crosstab()` สร้าง Cross-Tabulation (ตารางความถี่ร่วม)
- ใช้สีแสดงค่า — สีเข้ม = จำนวนมาก, สีอ่อน = น้อย

**พารามิเตอร์ที่ใช้:**
```python
ct = pd.crosstab(df["Gender"], df["Is_Member"])
plt.imshow(ct, cmap="coolwarm", interpolation="nearest")
```

**สิ่งที่เรียนรู้:**
- กลุ่มใดมีจำนวนลูกค้ามากที่สุด
- ความสัมพันธ์ระหว่าง Gender และ Is_Member

---

### 14. 🧱 Mosaic Plot — แสดงสัดส่วนแบบพื้นที่

**วัตถุประสงค์:** แสดงสัดส่วนของข้อมูลแบบ 2 มิติ (Gender × Is_Member) ด้วยพื้นที่ของสี่เหลี่ยม

**Concepts สำคัญ:**
- **Mosaic Plot** แสดงสัดส่วนของแต่ละกลุ่มเป็นพื้นที่ของสี่เหลี่ยม
- พื้นที่越大 = จำนวนมาก越
- เหมาะกับข้อมูลหมวดหมู่ 2 ตัวแปร

**พารามิเตอร์ที่ใช้:**
```python
from statsmodels.graphics.mosaicplot import mosaic
mosaic(df, ["Gender", "Is_Member"])
```

**สิ่งที่เรียนรู้:**
- เห็นสัดส่วนของแต่ละกลุ่มได้ชัดเจน
- เปรียบเทียบพื้นที่ระหว่างกลุ่มได้ง่าย

---

## สรุป (Summary)

### ตารางสรุป Visualization ทั้งหมด

| # | ชื่อ | ประเภทข้อมูล | Libraries |
|---|------|-------------|-----------|
| 1 | Histogram | Continuous (Income) | Matplotlib |
| 2 | Box Plot | Continuous (Income) | Matplotlib |
| 3 | Bar Chart (Frequency & Proportion) | Categorical (Gender) | Matplotlib |
| 4 | Scatter Plot (2D) | Continuous (Age, Income) | Matplotlib |
| 5 | Scatter Plot (Colored & Sized) | Continuous + Categorical | Matplotlib |
| 6 | 3D Scatter Plot (Matplotlib) | Continuous (3 vars) | Matplotlib |
| 7 | Interactive 3D Scatter Plot (Plotly) | Continuous + Categorical | Plotly |
| 8 | Correlation Heatmap | Continuous (3 vars) | Matplotlib |
| 9 | Pair Plot | Continuous (3 vars) | Matplotlib |
| 10 | Pie Chart | Categorical (Gender) | Matplotlib |
| 11 | Grouped Bar Chart | Categorical × Categorical | Matplotlib |
| 12 | Stacked Bar Chart | Categorical × Categorical | Matplotlib |
| 13 | Heatmap (Cross-Tab) | Categorical × Categorical | Matplotlib |
| 14 | Mosaic Plot | Categorical × Categorical | Statsmodels |

---

## เคล็ดลับ (Tips)

1. **เลือกกราฟให้เหมาะสมกับข้อมูล**
   - ข้อมูลต่อเนื่อง (Continuous) → Histogram, Scatter Plot, Box Plot
   - ข้อมูลหมวดหมู่ (Categorical) → Bar Chart, Pie Chart
   - ข้อมูลหลายตัวแปร → Pair Plot, Heatmap, 3D Scatter Plot

2. **ใช้สีอย่างมีความหมาย**
   - สีควรสื่อความหมาย (เช่น M = น้ำเงิน, F = ชมพู)
   - ใช้สีที่แตกต่างชัดเจนเพื่อให้คนตาบอดสีแยกแยะได้

3. **เพิ่ม Label และ Title**
   - ทุกกราฟควรมี Title, Label ของแกน, และ Legend
   - เพิ่มค่าตัวเลขบนกราฟเพื่อความชัดเจน

4. **Interactive vs Static**
   - ใช้ **Plotly** สำหรับ Interactive (นำเสนอ, Dashboard)
   - ใช้ **Matplotlib** สำหรับ Static (รายงาน, เอกสาร)

---

## อ้างอิง (References)

- [Matplotlib Documentation](https://matplotlib.org/stable/contents.html)
- [Pandas Documentation](https://pandas.pydata.org/docs/)
- [Plotly Documentation](https://plotly.com/python/)
- [Statsmodels Documentation](https://www.statsmodels.org/stable/index.html)

---

**สร้างด้วย ❤️ โดยใช้ Python, Pandas, Matplotlib, Plotly**
