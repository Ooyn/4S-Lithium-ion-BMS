# 4S-Lithium-ion-BMS
This repo contains a custom 4S Lithium-ion BMS using an active shunt architecture to safely charge and balance battery packs.  Each cell module pairs a TL431LP voltage reference with a BD140 transistor. When a cell reaches 4.2V, the circuit bypasses the current, preventing overvoltage while allowing the remaining cells to fully charge.
