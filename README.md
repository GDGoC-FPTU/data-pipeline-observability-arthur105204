[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=23573984&assignment_repo_type=AssignmentRepo)
# Day 10 Lab: Data Pipeline & Data Observability

**Student Email:** (update your email if required by instructor)
**Name:** Nguyen Manh Phu
**Student ID:** AI20K-202600178

---

## Mo ta

Bai lab nay implement ETL pipeline co 4 buoc:

1. Extract: doc du lieu tu `raw_data.json`
2. Validate: loai bo du lieu khong hop le (`price <= 0` hoac `category` rong)
3. Transform: tinh `discounted_price = price * 0.9`, chuan hoa `category` sang Title Case, them cot `processed_at`
4. Load: luu ket qua ra `processed_data.csv`

Ngoai ra, da chay stress test voi `agent_simulation.py` de so sanh hanh vi Agent khi dung clean data vs garbage data.

---

## Cach chay (How to Run)

### Prerequisites
```bash
pip install pandas pytest
```

### Chay ETL Pipeline
```bash
python solution.py
```

### Chay Agent Simulation (Stress Test)
```bash
python generate_garbage.py
python agent_simulation.py
```

### Terminal logs da ghi nhan
```text
(venv) PS ...\data-pipeline-observability-arthur105204> python .\solution.py
==================================================
ETL Pipeline Started...
==================================================
Validation summary: 3 kept, 2 dropped.
Errors found: [{'id': 3, 'reason': 'Price <= 0'}, {'id': 4, 'reason': 'Missing Category'}]
Successfully loaded 3 records to processed_data.csv

Pipeline completed! 3 records saved.

(venv) PS ...\data-pipeline-observability-arthur105204> python .\generate_garbage.py
garbage_data.csv has been created with 'Poisoned' records.

(venv) PS ...\data-pipeline-observability-arthur105204> python .\agent_simulation.py
Testing with CLEAN data:
Agent: Based on my data, the best choice is Laptop at $1200.

Testing with GARBAGE data:
Agent: Based on my data, the best choice is Nuclear Reactor at $999999.
```

---

## Cau truc thu muc

```
├── solution.py              # ETL Pipeline script
├── processed_data.csv       # Output cua pipeline
├── raw_data.json            # Input data
├── generate_garbage.py      # Script tao du lieu rac de stress test
├── garbage_data.csv         # Du lieu rac (generated)
├── agent_simulation.py      # Agent simulation script
├── experiment_report.md     # Bao cao thi nghiem
└── README.md                # File nay
```

---

## Ket qua

### 1) ETL output summary
- Tong so records input (`raw_data.json`): 5
- So records hop le sau validate: 3
- So records bi loai: 2
	- id=3: `Price <= 0`
	- id=4: `Missing Category`
- So records output (`processed_data.csv`): 3

### 2) Du lieu sau transform
`processed_data.csv` chua cac cot:
- `id`, `product`, `price`, `category`, `discounted_price`, `processed_at`

Vi du output:
- Laptop, price 1200 -> discounted_price 1080.0, category `Electronics`
- Chair, price 45 -> discounted_price 40.5, category `Furniture`
- Monitor, price 300 -> discounted_price 270.0, category `Electronics`

### 3) Stress test: Data quality impact

| Scenario | Agent response | Nhan xet |
|----------|----------------|----------|
| Clean data (`processed_data.csv`) | Agent chon **Laptop at $1200** | Hop ly voi business context |
| Garbage data (`garbage_data.csv`) | Agent chon **Nuclear Reactor at $999999** | Sai lech do outlier va du lieu doc |

### 4) Ket luan
- Data quality anh huong truc tiep den chat luong output cua Agent.
- Prompt tot khong bu duoc du lieu xau.
- ETL + validation la lop phong thu bat buoc truoc khi dua du lieu vao he thong AI.
