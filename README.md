# Sistem de comandă SCADA/PLC pentru linia de producție virtuală automatizată

**Autor:** Putnic Alexandru-Rareș
**Coordonator:** conf. dr. ing. Adrian Korodi
**Facultatea de Automatică și Calculatoare, Universitatea Politehnica Timișoara**

## 1. Repository

Adresa repository-ului:
`https://github.com/Neamtu2004/Proiectarea-sistemului-de-comand-i-SCADA_pentru_o_linie_automatizat-.git`

Repository-ul conține întregul cod sursă al proiectului (fără fișiere binare compilate):

- proiectul TIA Portal (fișiere sursă exportate, fără arhivele compilate `.zap*`)
- blocul de organizare principal și blocurile funcționale (Function Blocks) exportate ca surse (`.xml` / `.scl` / `.udt`), inclusiv:
  - `OB1 – Main` (programul ciclic principal, logica Start/Stop, Auto/Manual)
  - `FB1 – Machine Center Funtion`
  - `FB2 – Machine Center Base`
  - `FB3 – VisionSorter`
  - `FB4 – Assembly`
  - `FB5 – PaletLogic`
  - `FB6 – PaletLogic_1`
  - `FB7 – TurnTable`
  - `FB8 – Stacker Crane`
  - `FB9 – ConveyorStackerCrane`
  - `FB10 – Block_1`
- proiectul WinCC (ecrane HMI/SCADA)
- configurația serverului OPC UA
- fișierele scenei Factory I/O
- documentația tehnică (diagrame, schema I/O, capturi TIA Portal)

## 2. Structura repository-ului

```
/PLC
    /FB              -- blocurile funcționale (surse exportate din TIA Portal)
    /Tags            -- tabelele de variabile PLC
/SCADA
    /WinCC           -- proiectul WinCC (ecrane, alarme, tag-uri)
/OPCUA
    server-config.xml -- configurația serverului OPC UA
/FactoryIO
    scene.factoryio  -- scena Factory I/O
/docs
    schema_IO.pdf
    diagrame.pdf
README.md
```

## 3. Cerințe software

- TIA Portal (V17 sau ulterior)
- S7-PLCSIM Advanced (pentru simularea PLC-ului, dacă nu se folosește un PLC fizic)
- Factory I/O
- WinCC Runtime (Advanced sau Professional, conform proiectului)
- Server/Driver OPC UA compatibil (integrat în TIA Portal / Factory I/O)

## 4. Pași de compilare (build)

1. Deschide TIA Portal și creează un proiect nou sau importă proiectul din `/PLC`.
2. Importă blocurile funcționale (FB1–FB8) din `/PLC/FB` în proiect (clic dreapta pe *Program blocks* → *Import External Source* / *Import*).
3. Importă tabelele de variabile din `/PLC/Tags`.
4. Verifică ținta hardware configurată (CPU S7-1500 sau CPU compatibilă folosită în proiect).
5. Compilează proiectul: click dreapta pe stația PLC → **Compile → Software (rebuild all)**.
6. Corectează eventualele erori de compilare (referințe lipsă, tag-uri neconectate).

## 5. Pași de instalare și lansare a aplicației

### 5.1 Simulare PLC (fără hardware fizic)

1. Pornește **S7-PLCSIM Advanced**.
2. Din TIA Portal, încarcă (Download) proiectul compilat pe instanța PLCSIM (**Online → Download to device**, selectând PLCSIM ca țintă).
3. Pornește CPU-ul virtual (Run mode).

### 5.2 Configurare Factory I/O

1. Deschide scena din `/FactoryIO/scene.factoryio` în Factory I/O.
2. În meniul **File → Driver**, selectează driverul PLC corespunzător (Siemens S7-PLCSIM / S7-1500) și mapează intrările/ieșirile conform tabelului din `/docs/schema_IO.pdf`.
3. Pornește simularea (Run).

### 5.3 Server OPC UA

1. Activează serverul OPC UA din TIA Portal (Device configuration → Properties → OPC UA → Server), folosind configurația din `/OPCUA/server-config.xml`.
2. Verifică conexiunea folosind un client OPC UA (ex. UaExpert) pentru testare.

### 5.4 WinCC (SCADA)

1. Deschide proiectul WinCC din `/SCADA/WinCC`.
2. Configurează conexiunea (comunicarea) cu CPU-ul PLC (fizic sau PLCSIM).
3. Compilează proiectul HMI (**Compile**).
4. Lansează runtime-ul WinCC (**Start Runtime**).

### 5.5 Ordinea de pornire recomandată

1. S7-PLCSIM Advanced (sau PLC fizic) → Run
2. Download proiect TIA Portal pe CPU
3. Pornire Factory I/O + selectare driver
4. Pornire server OPC UA
5. Pornire runtime WinCC

## 6. Note

- Toate fișierele binare compilate (`.zap*`, executabile etc.) sunt excluse din repository conform cerinței; doar sursele sunt incluse.
- Pentru detalii suplimentare despre logica fiecărui bloc funcțional, consultă documentația tehnică din `/docs`.
