Sistema de Vínculo (Bond/Control) - Pokémon Essentials v21.1
Este módulo implementa una variable dinámica llamada bond_level (Vínculo) para cada Pokémon. Este valor influye en mecánicas de combate (probabilidad de críticos) y se gestiona mediante el desempeño en batalla.

Resumen de Modificaciones
1. Clase Pokemon (Script: Pokemon)
  Se ha modificado la estructura de los Pokémon para incluir y gestionar la nueva variable.

Definición de Variable: Se añadió bond_level como un attr_accessor para permitir su lectura y escritura.

Inicialización: Se añadió @bond_level = 100 en el método initialize. Todo Pokémon nuevo comienza con un valor base.

Métodos de Gestión (Seguridad):

def bond_level: Verifica si la variable es nil (útil para partidas guardadas antiguas) y la inicializa a 100 si es necesario.

def changeBondLevel(amount): Método para aumentar o disminuir el vínculo, limitando el valor (clamping) entre 0 y 255.

2. Mecánica de Combate (Scripts: Battle_Move y Move_UsageCalculations)
  El nivel de vínculo afecta directamente la capacidad del Pokémon para asestar golpes críticos.

Inyección en Cálculo de Daño: En pbIsCritical?, si el bond_level es >= 200, el golpe se convierte automáticamente en crítico o recibe una bonificación mayor.

Etapa de Crítico: En pbCalcCritStage, se añadió una comprobación para aumentar la etapa de crítico (crit_stage += 1) si el vínculo es alto.

3. Lógica de Post-Batalla (Script: Battle_End)
  El valor del vínculo fluctúa según el resultado del combate para cada Pokémon individual del equipo.

Aumento por Victoria:

Al ganar una batalla (@decision == 1 o 4), los Pokémon que no se han debilitado ganan puntos de vínculo.

Ganancia: +3 puntos en batallas salvajes, +5 puntos en batallas contra entrenadores.

Reducción por Debilitamiento:

Se implementó un bucle al final de pbEndOfBattle que verifica si algún Pokémon del jugador terminó con HP <= 0.

Penalización: -5 puntos de vínculo si el Pokémon cae debilitado.
