# Refactorización – TraxNY INC (Patrones GoF)

Este documento contiene el análisis, problemas detectados y justificación de patrones GoF aplicados para mejorar el diseño del sistema.

---

## 1. Problemas identificados (mínimo 10)

| # | Problema detectado | Categoría / Falta de patrón |
|---|---------------------|-----------------------------|
| 1 | Uso de `if`/`else` o `switch` para seleccionar productos | ❌ Violación OCP — usar Factory Method / Strategy |
| 2 | `ProductionManager` tiene múltiples responsabilidades | ❌ Violación SRP — aplicar Command |
| 3 | Acoplamiento fuerte entre manager y productos concretos | ❌ Falta de abstracción — aplicar Abstract Factory |
| 4 | Lógica duplicada en los flujos de producción | ❌ Oportunidad Template Method |
| 5 | Uso de strings mágicos (`"gold"`, `"diamond"`) | ❌ Código frágil — usar Enum Strategy |
| 6 | No existe interfaz común para productos | ❌ Falta de polimorfismo — crear `IProductOperation` |
| 7 | Métodos como `Grow()`, `Forge()` sin encapsulación | ❌ Aplicar Command |
| 8 | No existe registro de eventos | ❌ Falta Observer / Mediator |
| 9 | Dependencia directa a `Console.WriteLine` | ❌ Aplicar Adapter |
|10 | Agregar un nuevo producto rompe el método `Produce()` | ❌ Violación OCP — Factory Method |
|11 | Violación DIP (manager depende de clases concretas) | ❌ Aplicar interfaces |
|12 | Código difícil de testear (salida fija en consola) | ❌ Adapter + inyección de dependencias |

---

## 2. Justificación de problemas y patrones aplicados

### 1. Condicionales para crear productos → **Factory Method**
Los `if/switch` hacían el código rígido y no extensible.

**Solución:**  
Usar `ProductFactory` para crear productos según `ProductType`.

---

### 2. ProductionManager con demasiadas responsabilidades → **Command**
Controlaba pasos, creación, impresión y registro.

**Solución:**  
Encapsular acciones en comandos independientes.

---

### 3. Acoplamiento fuerte → **Abstract Factory**
`new GoldIngot()` dentro del manager violaba DIP.

**Solución:**  
Crear `IProductFactory` que retorna `IProductOperation`.

---

### 4. Flujo repetido entre productos → **Template Method**
Varios productos repetían los mismos pasos.

**Solución:**  
Crear plantilla general del proceso con pasos modificables.

---

### 5. Strings mágicos → **Enum Strategy**
Los tipos `"gold"`, `"diamond"` eran propensos a errores.

**Solución:**  
Enum seguro: `ProductType`.

---

### 6. Sin interfaz base para productos → **Polimorfismo**
Cada producto tenía métodos diferentes.

**Solución:**  
Crear interfaz común:

```csharp
public interface IProductOperation {
    void ExecuteProcess();
}
```
### 7. Métodos sueltos → Command

Operaciones encapsuladas y reutilizables.

### 8. Sin sistema de eventos → Observer / Mediator

Nada notificaba cambios.

**Solución:**
Capa IOutput que recibe eventos.

### 9. Dependencia a consola → Adapter

El código usaba directamente Console.WriteLine.

**Solución:**
ConsoleOutputAdapter implementa IOutput.

### 10. Nuevos productos rompen el flujo → Factory Method

Sin fábrica, había que modificar código.

**Solución:**
OCP asegurado gracias a la fábrica.

### 11. Violación DIP → Interfaces

El manager dependía de clases concretas.

**Solución:**
Inyección de IProductFactory y IOutput.

### 12. Difícil de testear → Adapter

Salida desacoplada para pruebas unitarias.
