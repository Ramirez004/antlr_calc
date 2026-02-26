# 🧮 Implementación de la Calculadora con ANTLR4 (Java)

## 📘 Basado en:
**The Definitive ANTLR 4 Reference – Terence Parr**  
Capítulo 4 – Building a Calculator

---

# 1️⃣ Primera fase: Generación básica del parser

Se inicia con la gramática básica `Expr.g4`:

```bash
antlr4 Expr.g4
```

Este comando genera automáticamente los siguientes archivos:

```
ExprBaseListener.java
ExprListener.java
ExprLexer.java
ExprParser.java
```

Verificación:

```bash
ls Expr*.java
```

---

# 2️⃣ Compilación del código generado

Una vez generados los archivos Java, se compilan:

```bash
javac Expr*.java
```

En este punto ya tenemos:

- Lexer funcional
- Parser funcional
- Listener generado automáticamente

---

# 3️⃣ Visualización del Árbol Sintáctico

Para probar el parser sin escribir código adicional, se usa la herramienta `grun`:

```bash
grun Expr prog -gui t.expr
```

Donde `t.expr` contiene:

```txt
3+4
```

Esto abre una ventana gráfica mostrando el árbol sintáctico.

<img width="1608" height="885" alt="Captura desde 2026-02-26 08-10-43" src="https://github.com/user-attachments/assets/4cceeb97-d082-4260-a2c4-2592754b6772" />


---

# 4️⃣ Creación de un programa Java para ejecutar el parser

Siguiendo el libro, se implementa `ExprJoyRide.java` para ejecutar el parser desde código Java:

```bash
javac ExprJoyRide.java Expr*.java
```

Ejecución:

```bash
java ExprJoyRide t.expr
```

En esta etapa:

- Se crea manualmente el `CharStream`
- Se instancia el `Lexer`
- Se instancia el `Parser`
- Se obtiene el árbol (`ParseTree`)
- Se imprime el árbol con `toStringTree()`

---

# 5️⃣ Evolución hacia una calculadora real (Visitor Pattern)

El libro introduce el patrón **Visitor** para evaluar expresiones.

Se crea una nueva gramática con etiquetas:

```bash
antlr4 -no-listener -visitor LabeledExpr.g4
```

Esto genera:

```
LabeledExprLexer.java
LabeledExprParser.java
LabeledExprBaseVisitor.java
LabeledExprVisitor.java
```

El uso de `-visitor` permite:

- Implementar evaluación personalizada
- Separar la lógica semántica del parser
- Aplicar el patrón Visitor correctamente

---

# 6️⃣ Implementación de EvalVisitor

Se implementa la clase `EvalVisitor.java`, extendiendo:

```java
LabeledExprBaseVisitor<Integer>
```

Se sobrescriben métodos como:

- `visitAddSub`
- `visitMulDiv`
- `visitInt`
- `visitId`
- `visitAssign`

Esto permite:

✔ Evaluar operaciones aritméticas  
✔ Manejar precedencia  
✔ Soportar variables  
✔ Soportar paréntesis  

---

# 7️⃣ Programa principal definitivo: Calc.java

Se implementa:

```bash
javac Calc.java EvalVisitor.java LabeledExpr*.java
```

Ejecución:

```bash
java Calc prueba1.expr
```

---

# 8️⃣ Inclusión de reglas compartidas (CommonLexerRules)

Siguiendo el libro, se puede reutilizar reglas léxicas:

```bash
antlr4 LibExpr.g4
```

Donde `LibExpr.g4` importa automáticamente:

```
CommonLexerRules.g4
```

Esto permite:

- Reutilización de reglas léxicas
- Modularización de gramáticas
- Mejor organización del proyecto

---

# 9️⃣ Pruebas Finales

Archivo: `prueba1.expr`

```txt
3+4
10-2
5*6
20/4
a=5
a+3
(2+3)*4
```

Ejecución:

```bash
java Calc prueba1.expr
```

El sistema:

- Analiza el archivo línea por línea
- Construye el árbol sintáctico
- Evalúa las expresiones
- Imprime los resultados

---

# 🌳 Árbol Sintáctico Final

Visualización:

```bash
grun LabeledExpr prog -gui prueba1.expr
```
<img width="1663" height="938" alt="antlr4_parse_tree" src="https://github.com/user-attachments/assets/2e6ff248-86df-4e02-9483-f54962ce6ffa" />



---

# 🔎 Conclusión del Proceso

Durante la implementación se comprendieron los siguientes conceptos fundamentales del libro:

- Separación entre Lexer y Parser
- Generación automática de código
- Uso de `grun` para depuración
- Construcción y visualización del Parse Tree
- Diferencia entre Listener y Visitor
- Aplicación del patrón Visitor
- Evaluación semántica sobre el árbol
- Modularización de gramáticas

El proyecto evolucionó desde:

✔ Generar un árbol  
➡ Visualizarlo  
➡ Ejecutarlo desde Java  
➡ Evaluarlo  
➡ Convertirlo en una calculadora funcional  

---

# ✅ Estado Final

✔ Parser funcional  
✔ Árbol sintáctico visualizable  
✔ Evaluación aritmética completa  
✔ Manejo de variables  
✔ Implementación siguiendo el libro oficial  

---
