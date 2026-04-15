# Experiment Report: Data Quality Impact on AI Agent

**Student ID:** AI20K-2A202600178
**Name:** Nguyen Manh Phu
**Date:** 2026-04-15

---

## 1. Ket qua thi nghiem

Chay `agent_simulation.py` voi 2 bo du lieu va ghi lai ket qua:

| Scenario | Agent Response | Accuracy (1-10) | Notes |
|----------|----------------|-----------------|-------|
| Clean Data (`processed_data.csv`) | Agent: Based on my data, the best choice is Laptop at $1200. | 10/10 | Ket qua hop ly, khong co outlier doc hai, category da duoc chuan hoa |
| Garbage Data (`garbage_data.csv`) | Agent: Based on my data, the best choice is Nuclear Reactor at $999999. | 1/10 | Agent bi dinh huong sai boi outlier gia cuc cao va du lieu nhieu loi |

Tom tat run log:
- ETL pipeline: `Validation summary: 3 kept, 2 dropped`
- Dropped records:
	- `id=3` vi `Price <= 0`
	- `id=4` vi `Missing Category`
- Output clean duoc luu vao `processed_data.csv` voi 3 records hop le.

---

## 2. Phan tich & nhan xet

### Tai sao Agent tra loi sai khi dung Garbage Data?

Agent simulation trong bai nay chon san pham dien tu co gia cao nhat trong tap du lieu. Khi dung garbage data, tap du lieu co nhieu van de chat luong nhu duplicate IDs (id=1 lap lai), wrong data types (`price="ten dollars"`), outlier cuc lon (`Nuclear Reactor` gia `999999`), va null values (id/category rong). Trong do, outlier gia 999999 co tac dong manh nhat vi logic chon `idxmax` se uu tien ban ghi nay, lam ket qua tra loi sai nghiem trong ve nghiep vu. Duplicate va wrong type lam du lieu kho tin cay, de gay loi xu ly trong cac buoc thong ke/loc. Null values lam giam do phu du lieu va co the gay route sai neu system can category hop le de phan loai. Ket luan: neu khong co lop ETL validation + cleaning truoc khi cho Agent truy cap du lieu, output de bi nhieu va hallucination theo huong "data-induced".

---

## 3. Ket luan

**Quality Data > Quality Prompt?** (Dong y hay khong? Giai thich ngan gon.)

Dong y. Quality data quan trong hon quality prompt trong pipeline nay. Prompt co the ro rang, nhung neu input data bi nhiem (outlier, wrong type, null, duplicate) thi model/agent van ra quyet dinh sai. Prompt chi huong dan cach tra loi, con data moi quyet dinh chat luong noi dung tra loi. Vi vay ETL, validation, observability va data governance la dieu kien tien quyet de AI system on dinh va dang tin cay.
