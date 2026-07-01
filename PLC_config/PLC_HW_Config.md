

## 1. CPU recomandată

Proiectul folosește o linie de producție simulată integral în **Factory
I/O**, controlată prin **S7-PLCSIM Advanced**. Factory I/O comunică cu
CPU-ul direct prin driverul său nativ Siemens S7-1500/S7-PLCSIM — **nu
sunt necesare module I/O fizice**, driverul scrie/citește direct în zona
de proces (%I / %Q) a CPU-ului simulat.

- **Familie CPU:** SIMATIC S7-1500
- **Model recomandat:** CPU 1516-3 PN/DP (sau orice CPU S7-1500 cu
  suficientă memorie de proces pentru adresele folosite — vezi mai jos)
- **Interfață PLCSIM:** PLCSIM Advanced, instanță virtuală conectată la
  Factory I/O prin driverul Siemens S7-1500 (TCP, `localhost` sau IP
  virtual configurat în Factory I/O → Drivers)
- **Firmware:** V2.9 sau ulterior (compatibil cu funcțiile IEC folosite:
  `TON`, `TP`, `CTU`, `CTD`, `R_TRIG`, `N_TRIG`, `SR`)
- **Adresă IP PLC (PROFINET/PLCSIM):** `192.168.0.10`

## 2. Zona de memorie de proces necesară

Pe baza adreselor folosite în program (extrase din OB1 și FB-uri),
zona minimă de proces trebuie să acopere:

| Zonă | Interval folosit | Observații |
|------|-------------------|------------|
| Intrări digitale (%I) | I0.0 – I8.3 | ~68 biți (9 bytes) |
| Ieșiri digitale (%Q) | Q0.0 – Q11.0 | ~89 biți (12 bytes) |
| Memorie internă (%M) | M0.0 – M2.0 | flag-uri Start/Stop/Manual |
| Intrări analogice (%ID) | ID20 – ID60 | poziții X/Y/Z, senzori de lumină |
| Ieșiri analogice (%QD) | QD20 – QD72 | setpoint-uri poziție, viteze conveioare |
| Ieșiri analogice (%QW) | QW12, QW14 | locuri libere / procent ocupare raft |

> CPU-urile S7-1500 alocă implicit suficientă memorie de proces (de
> regulă 8 KB I + 8 KB Q în configurația standard), deci nu e nevoie de
> ajustări speciale — se verifică doar în TIA Portal, **Device
> configuration → Properties → System constants / I/O addresses**, ca
> intervalele de mai sus să nu fie ocupate de altceva.

## 3. Harta completă de adrese I/O (reconstruită din program)

### 3.1 Intrări digitale (%I)

| Adresă | Simbol | Bloc / utilizare |
|--------|--------|-------------------|
| I0.0 | FActoryIO Running | semnal general „scena Factory I/O rulează" |
| I0.1 | 1- SensorIntrare | Machine Center 1 |
| I0.2 | 1- SensorIesire | Machine Center 1 |
| I0.3 | 1- CenterError | Machine Center 1 |
| I0.4 | 1 -CenterBusy | Machine Center 1 |
| I0.5 | 2 -CenterBusy | Machine Center 2 |
| I0.6 | 2- CenterError | Machine Center 2 |
| I0.7 | 2- SensorIesire | Machine Center 2 |
| I1.0 | 2- SensorIntrare | Machine Center 2 |
| I1.1 | 3 -CenterBusy | Machine Center 3 |
| I1.2 | 3- CenterError | Machine Center 3 |
| I1.3 | 3- SensorIesire | Machine Center 3 |
| I1.4 | 3- SensorIntrare | Machine Center 3 |
| I1.5 | 4 -CenterBusy | Machine Center 4 |
| I1.6 | 4- CenterError | Machine Center 4 |
| I1.7 | 4- SensorIesire | Machine Center 4 |
| I2.0 | 4- SensorIntrare | Machine Center 4 |
| I2.1 | Panel Principal Start Button | panou general |
| I2.2 | Panel Principal Stop Button | panou general |
| I2.3 | Panel Principal Reset Button | panou general |
| I3.1 | Capacitive Sensor0 | Assembly 1 |
| I3.2 | Capacitive Sensor1 | Assembly 1 |
| I3.3 | RightLimit0 | Assembly 1 |
| I3.4 | RightLimit1 | Assembly 1 |
| I3.5 | Two-Axis Pick & Place 0 (Detected) | Assembly 1 |
| I3.6 | Right Positioner 0 (Limit) | Assembly 1 |
| I3.7 | Capacitive Sensor2 | Assembly 2 |
| I4.0 | Capacitive Sensor3 | Assembly 2 |
| I4.1 | LeftLimit2 | Assembly 2 |
| I4.2 | leftLimit3 | Assembly 2 |
| I4.3 | Two-Axis Pick & Place 1 (Detected) | Assembly 2 |
| I4.4 | Left Positioner 1 (Limit) | Assembly 2 |
| I4.5 | DiffuseSensorPalet | PaletLogic |
| I4.6 | DiffuseSensorPalet1 | PaletLogic |
| I4.7 | DiffuseSensorPalet2 | PaletLogic_1 |
| I5.0 | DiffuseSensorPalet3 | PaletLogic_1 |
| I5.1 | DiffuseSensorPickUp1 | PaletLogic |
| I5.2 | Pick & Place 1 (Box Detected) | PaletLogic |
| I5.3 | DiffuseSensorPickUp2 | PaletLogic_1 |
| I5.4 | Pick & Place 0 (Box Detected) | PaletLogic_1 |
| I5.5 | Retroreflective Sensor 10 | TurnTable |
| I5.6 | Turntable 0 (Back Limit)_1 | TurnTable |
| I5.7 | Retroreflective Sensor 9 | TurnTable |
| I6.0 | Retroreflective Sensor 8 | TurnTable |
| I6.1 | Turntable 1 (Back Limit) | TurnTable |
| I6.2 | Retroreflective Sensor 11 | TurnTable |
| I6.3 | Turntable 0 (Limit 90) | TurnTable |
| I6.4 | At_load | Stacker Crane / ConveyorStackerCrane |
| I6.5 | At_Right | Stacker Crane |
| I6.6 | At_Left | Stacker Crane |
| I6.7 | At_Middle | Stacker Crane |
| I7.0 | Moving_X | Stacker Crane |
| I7.1 | Moving_Z | Stacker Crane |
| I7.2 | At_entry | ConveyorStackerCrane |
| I7.3 | At_UnLoad | Stacker Crane / ConveyorStackerCrane |
| I7.4 | At_Exit | ConveyorStackerCrane |
| I7.5 | Turntable 1 (Front Limit) | TurnTable |
| I7.6 | Turntable 1 (Limit 90) | TurnTable |
| I7.7 | Turntable 1 (Limit 0) | TurnTable |
| I8.0 | Retroreflective Sensor 16 | TurnTable |
| I8.1 | Unload | Stacker Crane (comandă panou) |
| I8.2 | Load | Stacker Crane (comandă panou) |
| I8.3 | Manual_Mode | Stacker Crane |

### 3.2 Ieșiri digitale (%Q)

| Adresă | Simbol | Bloc / utilizare |
|--------|--------|-------------------|
| Q0.0 – Q0.2 | 1 - Conveyor Intrare / StartButton / Conveyor Iesire | Machine Center 1 |
| Q0.3 – Q0.5 | 2 - Conveyor Intrare / StartButton / Conveyor Iesire | Machine Center 2 |
| Q0.6 – Q1.0 | 3 - Conveyor Intrare / StartButton / Conveyor Iesire | Machine Center 3 |
| Q1.1 – Q1.3 | 4 - Conveyor Intrare / StartButton / Conveyor Iesire | Machine Center 4 |
| Q1.7 | Conveyor Centru4 | conveioare centru |
| Q2.0 | Conveyor Centru5 | conveioare centru |
| Q2.1 | Conveyor Centru6 | conveioare centru |
| Q2.2 – Q2.7 | 1..3 ResetButton / StopButton | Machine Centers 1-3 |
| Q3.0 – Q3.1 | 4-StopButton / 4-ResetButton | Machine Center 4 |
| Q3.2 – Q3.5 | Pusher1..Pusher4 | VisionSorter |
| Q3.6 – Q4.6 | ConveyorPusher / Pusher1-3 / Remover / Assembly / Assembly1-3 | conveioare principale |
| Q4.7 – Q6.6 | Right/Left Positioner, Two-Axis Pick & Place X/Z/Grab, RollerStop | Assembly 1 și 2 |
| Q6.7 – Q9.2 | ConveyotPalet1-6, EmitterPalet1-2, Pick&Place Set Points | PaletLogic / PaletLogic_1 |
| Q9.3 – Q11.0 | Turntable 0/1 Roll/Turn, ConveyorTurnTable, ConveyorPalet3 | TurnTable |
| Q10.1 – Q10.7 | Fork_Right/Left, Lift, Entry/Load/Unload/Exit Conveyor | Stacker Crane / ConveyorStackerCrane |

### 3.3 Memorie internă (%M) — flag-uri de comandă

| Adresă | Simbol |
|--------|--------|
| M0.0 | StartButton Memory |
| M0.2 | Load1 |
| M0.3 | Unload1 |
| M0.4 | ErrorMachine2 |
| M0.5 | ErrorMachine1 |
| M0.6 | Manual_Conveyor |
| M0.7 | Manual_Assembly1 |
| M1.0 | Manual_Mode_Global |
| M1.1 – M1.4 | Manual_MC01 – Manual_MC04 |
| M1.5 – M1.6 | Manual_PickPlace1 / Manual_PickPlace2 |
| M2.0 | Manual_Assembly2 |

### 3.4 Intrări/ieșiri analogice (%ID / %QD / %QW)

| Adresă | Simbol | Bloc |
|--------|--------|------|
| ID20 | VisionSensor | VisionSorter |
| ID24, ID28, ID32 | Pick & Place 1 X/Y/Z Position (V) | PaletLogic |
| ID36 | Light Array Emitter 0 (Value) | PaletLogic |
| ID40 | Light Array Emitter 2 (Value) | PaletLogic |
| ID44 | Light Array Emitter 3 (Value) | PaletLogic_1 |
| ID48, ID52, ID56 | Pick & Place 0 X/Y/Z Position (V) | PaletLogic_1 |
| ID60 | Light Array Emitter 1 (Value) | PaletLogic_1 |
| QD20, QD24, QD28, QD32 | Contor Machine Center 1-4 | Machine Centers |
| QD36, QD40, QD44 | Pick & Place 1 X/Y/Z Set Point (V) | PaletLogic |
| QD48, QD52, QD56 | Pick & Place 0 X/Y/Z Set Point (V) | PaletLogic_1 |
| QD60 | TargetPosition | Stacker Crane |
| QD64, QD68, QD72 | Conveyor Centru1/2/3 (viteză) | conveioare centru |
| MD64 | VitezaConveyor | setare manuală viteză |
| QW12 | Locuri Neocupate | Stacker Crane |
| QW14 | ProcentOcupare | Stacker Crane |

## 4. Pași de configurare în TIA Portal

1. **Add new device** → SIMATIC S7-1500 → selectează CPU-ul dorit (ex.
   CPU 1516-3 PN/DP) sau varianta **unspecified/PLCSIM** dacă lucrezi
   exclusiv în simulare.
2. Nu adăuga module I/O fizice — proiectul folosește exclusiv
   simbolurile din tabelul de mai sus, scrise/citite direct de Factory
   I/O prin driver.
3. În **Factory I/O → File → Driver**, selectează driverul Siemens
   S7-1500 / PLCSIM, setează adresa `192.168.0.10` și mapează fiecare
   element din scenă la adresa corespunzătoare din tabelul de mai sus.
4. Activează **PLCSIM Advanced** ca țintă de download pentru CPU.
