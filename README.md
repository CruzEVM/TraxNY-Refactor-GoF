# 🪙 TraxNY, Inc. – Refactorización con Patrones GoF

Práctica de **Refactorización y Diseño con Patrones GoF** para el sistema de manufactura de joyas de la empresa ficticia **TraxNY, Inc.**

El proyecto parte de un código inicial con problemas de diseño (acoplamiento, uso de `if` por tipo, ausencia de interfaces, dependencia directa a la consola, etc.) y propone una versión refactorizada aplicando patrones del catálogo GoF.

---

## 🎯 Objetivo de la práctica

- Identificar **defectos de diseño** en el código base.
- Aplicar **patrones GoF** y principios **SOLID** para mejorar:
  - Flexibilidad ante nuevos productos.
  - Mantenibilidad y legibilidad.
  - Posibilidad de pruebas unitarias.
- Simular la operación del sistema con:
  - Una **aplicación de consola** en C#.
  - Un **FrontEnd estático** que representa el panel de control de producción.

---

## 🧱 Código base (antes del refactor)

El código original se centraba en la clase `ProductionManager`:

```csharp
public class ProductionManager {
    public void Produce(string itemType) {
        if (itemType == "gold") {
            var g = new GoldIngot();
            g.StartMelting();
            g.StopMelting();
        } else if (itemType == "diamond") {
            var d = new DiamondLab();
            d.Grow();
            d.Analyze();
        } else if (itemType == "chain") {
            var c = new Chain();
            c.Forge();
            c.TestResistance();
        } else {
            Console.WriteLine("Unknown product type.");
        }
    }
}
