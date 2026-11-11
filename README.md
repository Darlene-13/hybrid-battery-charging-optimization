# Fast-Charging an EV Battery Cell: Hybrid Modelling & Optimization

Case Study 5 for *Machine Learning Use Cases, Weeks 8 to 9*. Presenter: Darlene Wekesa.

The project asks one question: can we charge an EV battery cell faster without breaking its voltage, temperature or aging limits? It compares three ways of modelling the cell and then uses the best one to find a safe, fast charging profile.

| Model | Idea | Weakness |
|---|---|---|
| Physics only | `V = OCV(SOC) + I*R0` plus a heat balance | One fixed resistance, so it is biased for aged or cold cells |
| ML only | Gradient boosting predicts voltage and heating directly | Cannot extrapolate to charge rates it never saw |
| Hybrid | Keep the physics equations; ML predicts only the extra resistance `dR` from SOC, cell temperature and cycle count | Still limited by the training range in temperature and cycles |

## Files

| File | What it is |
|---|---|
| `ev_charging_hybrid_modelling.ipynb` | The Colab/Jupyter notebook: load data, three models, comparison, optimization, limitations |
| `ev_cell_charging_data.xlsx` | Synthetic data. Sheets: `Parameters`, `OCV_Table`, `Train_Data` (2,000 rows, 0.2 to 1.5 C), `Test_Extrapolation` (500 rows, 2 to 3 C) |
| `ev_charging_hybrid_modelling_polished.pptx` | The presentation  |

## Running the notebook

Requires Python 3 with `numpy`, `pandas`, `matplotlib`, `scikit-learn`, `scipy` and `openpyxl`.

```bash
pip install numpy pandas matplotlib scikit-learn scipy openpyxl jupyter
jupyter notebook ev_charging_hybrid_modelling.ipynb
```

## Key results (from the notebook)

Voltage RMSE (mV), lower is better:

| Model | Inside training range | Unseen 2 to 3 C |
|---|---|---|
| Physics only | 88.3 | 177.6 |
| ML only | 15.7 | 192.4 |
| Hybrid | 6.6 | 39.0 |

Optimized 10 to 80% charge, aged cell (400 cycles, 25 °C), checked on the hidden "real cell":

| Profile | Time | Safe on real cell? |
|---|---|---|
| 1 C constant | 42.2 min | Yes |
| Optimized on physics only | 26.2 min | No (4.22 V, 1.08x degradation cap) |
| Optimized on hybrid | 28.5 min | Yes |

## Limitations

- The data is synthetic, and the "real cell" is a formula written for this demo. The hybrid's correction has the same shape as that formula, so it has an unfair advantage.
- ML-only uses a tree model, which cannot extrapolate by design; a neural network would be a fairer comparison.
- The hybrid still cannot extrapolate in cycle count or temperature.
- The circuit and thermal model are simplified (no hysteresis, RC dynamics or lithium plating), and the degradation measure is a proxy, not a validated aging model.
- Results come from one scenario.

## Contributor
Darlene Wendy

## Talk format (1 hour)
