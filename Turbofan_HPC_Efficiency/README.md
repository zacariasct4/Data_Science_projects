## Variables (Turbofan / HPC efficiency)

This dataset contains turbofan performance and cycle variables used to predict **Isentr.HPCEfficiency** (isentropic efficiency of the High-Pressure Compressor, HPC).

### Target
- **Isentr.HPCEfficiency (float)**: Isentropic efficiency of the high-pressure compressor (how close the real compression process is to an ideal isentropic process). Higher is better.

### Thrust & propulsive performance
- **NetThrust_kN (float)**: Total net thrust produced by the engine (kN).
- **CoreNozzleGrossThrust_kN (float)**: Gross thrust contribution from the core (hot stream) nozzle (kN).
- **BypassNozzleGrossThrust_kN (float)**: Gross thrust contribution from the bypass (cold stream) nozzle (kN).
- **Sp.FuelConsumption_g/(kN*s) (float)**: Thrust Specific Fuel Consumption (TSFC), i.e., grams of fuel per kN of thrust per second. Lower is better.
- **SpecificThrust_m/s (float)**: Thrust per unit air mass flow (often reported in m/s in engine datasets).

### Nozzle flow variables
- **CoreNozzleVel.V8_m/s (float)**: Exhaust jet velocity at the core nozzle exit (station V8), in m/s.
- **CoreNozzlePressureRatio (float)**: Pressure ratio across/at the core nozzle (nozzle pressure ratio).
- **BypassNozzleVel.V18_m/s (float)**: Exhaust jet velocity at the bypass nozzle exit (station V18), in m/s.
- **BypassNozzlePressureRatio (float)**: Pressure ratio across/at the bypass nozzle (nozzle pressure ratio).

### Combustion & overall pressure ratios
- **BurnerEfficiency (float)**: Combustor (burner) efficiency; fraction of chemical energy effectively transferred to the flow.
- **EnginePressureRatioP5/P2 (float)**: Engine Pressure Ratio (EPR), defined here as the pressure ratio between station P5 and P2.

### Spools & fuel flow
- **HPSpoolSpeed_RPM (float)**: High-pressure spool rotational speed (RPM).
- **LPSpoolSpeed_RPM (float)**: Low-pressure spool rotational speed (RPM).
- **FuelFlow_kg/s (float)**: Fuel mass flow rate (kg/s).

### Low-pressure turbine (LPT) exit conditions
- **LPTExitPressureP5_kPA (float)**: Pressure at the exit of the low-pressure turbine (station P5), in kPa.
- **LPTExitTemperatureT5_K (float)**: Temperature at the exit of the low-pressure turbine (station T5), in K.
