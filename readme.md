# Sustentación Interactiva: Simulación de Datapath RISC-V (RV32I)

Este repositorio contiene la implementación y preparación para la sustentación interactiva de la microarquitectura de un procesador **RISC-V (RV32I)** de ciclo único (monociclo).

---

## 📋 1. ¿Qué se hizo? (Propósito del Trabajo)
Se diseñó un programa no trivial en **C++** que implementa el **cálculo del factorial de un número de forma recursiva**. Posteriormente, dicho programa fue compilado a lenguaje ensamblador **RISC-V base (`RV32I`)** para analizar y demostrar el comportamiento físico de los bloques de hardware (Ruta de Datos y Unidad de Control) durante su ejecución en el simulador visual.

### Objetivos clave:
* Demostrar la interacción de instrucciones Tipo R, Tipo I, Tipo S y Tipo B dentro del procesador monociclo.
* Analizar la gestión de la memoria de pila (*Stack*) en llamadas a funciones recursivas (`sw`, `lw`, `addi sp, sp, -offset`).
* Explicar el flujo de señales paso a paso (Fetch, Decode, Execute, Memory, Writeback) durante la simulación oral con el profesor.

---

## 🛠️ 2. ¿Cómo se hizo? (Metodología y Herramientas)

1. **Desarrollo del Algoritmo en C++:**
   Se escribió la función recursiva del factorial priorizando estructuras de control (`if`) y manejo de pila.

2. **Compilación a Ensamblador (`RV32I`):**
   Se utilizó la herramienta **Compiler Explorer (Godbolt)** con la siguiente configuración:
   * **Lenguaje:** `C++`
   * **Compilador:** `RISC-V (32-bits) gcc`
   * **Banderas de compilación (Compiler Options):**
     ```text
     -O0 -mabi=ilp32 -march=rv32i
     ```
     *(Nota: `-O0` desactiva optimizaciones para conservar cada instrucción explícita y `-march=rv32i` fuerza el uso del conjunto de instrucciones base de 32 bits).*

3. **Carga en el Simulador Visual:**
   El código ensamblador obtenido se cargó en el simulador gráfico de RISC-V para rastrear los datos en los registros (`sp`, `ra`, `a0`, `s0`), la ALU y la memoria de datos.

---

## 📄 3. Código Fuente

### Código en C++
```cpp
int factorial(int n) {
    if (n <= 1) {
        return 1;
    }
    return n * factorial(n - 1);
}

int main() {
    int numero = 4;









RESULTADO DEL EMSAMBLADOR  
factorial:
    addi    sp, sp, -16
    sw      ra, 12(sp)
    sw      s0, 8(sp)
    addi    s0, sp, 16
    sw      a0, -12(s0)
    
    lw      a0, -12(s0)
    li      a5, 1
    ble     a0, a5, .L2
    
    lw      a0, -12(s0)
    addi    a0, a0, -1
    call    factorial
    
    # Manejo del retorno de la multiplicación recursiva
    lw      a5, -12(s0)
    # Note: En hardware se completa el producto con subrutina o acumulador
    j       .L3

.L2:
    li      a0, 1

.L3:
    lw      ra, 12(sp)
    lw      s0, 8(sp)
    addi    sp, sp, 16
    ret

main:
    addi    sp, sp, -16
    sw      ra, 12(sp)
    sw      s0, 8(sp)
    addi    s0, sp, 16
    li      a0, 4
    call    factorial
    lw      ra, 12(sp)
    lw      s0, 8(sp)
    addi    sp, sp, 16
    ret
    int resultado = factorial(numero);
    return resultado;
}
