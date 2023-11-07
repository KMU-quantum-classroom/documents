# Tutorial

Examples of how to use the package to convert different expressions, such as quantum circuits to matrices, matrices to quantum circuits, and quantum circuits to bra-ket notations. 

It also shows how to pass options to the conversion service, such as expression simplification or expansion.

## Before you start

```Python
from qiskit import QuantumCircuit, transpile
from qiskit_aer import AerSimulator

from qiskit_class_converter import ConversionService, ConversionType
```

Declare required qiskit_class_converter packages.

## matrix to quantum circuit

```Python
input_value = [
[1, 0, 0, 0],
[0, 0, 0, 1],
[0, 0, 1, 0],
[0, 1, 0, 0]
]

sample_converter = ConversionService(conversion_type="MATRIX_TO_QC", option={"label": "CX gate"})
result = sample_converter.convert(input_value=input_value)
quantum_circuit = QuantumCircuit(2, 2)
quantum_circuit.x(0)
quantum_circuit.append(result, [0, 1])
quantum_circuit.measure(range(2), range(2))
backend = AerSimulator()
qc_compiled = transpile(quantum_circuit, backend)
counts = backend.run(qc_compiled).result().get_counts()
```

You will get the variables : quantum_circuit, qc_compiled's counts

* quantum_circuit
```Text
text : 
     ┌───┐┌──────────┐┌─┐   
q_0: ┤ X ├┤0         ├┤M├───
     └───┘│  CX gate │└╥┘┌─┐
q_1: ─────┤1         ├─╫─┤M├
          └──────────┘ ║ └╥┘
c: 2/══════════════════╩══╩═
                       0  1   
```

* qc_compiled's counts
```Python
  
{'11': 1024}
```

## quantum circuit to matrix

```Python
quantum_circuit = QuantumCircuit(2, 2)
quantum_circuit.x(0)
quantum_circuit.cx(0, 1)
sample_converter = ConversionService(conversion_type="QC_TO_MATRIX")
var = sample_converter.convert(input_value=quantum_circuit)
```

You will get the variables : ```var["gate"]```, ```var["name"]```, ```var["result"]```

```var["gate"]``` : gate matrix list values before are computed.

```Python
[array([[0.+0.j, 1.+0.j, 0.+0.j, 0.+0.j],
       [1.+0.j, 0.+0.j, 0.+0.j, 0.+0.j],
       [0.+0.j, 0.+0.j, 0.+0.j, 1.+0.j],
       [0.+0.j, 0.+0.j, 1.+0.j, 0.+0.j]]),
 array([[1.+0.j, 0.+0.j, 0.+0.j, 0.+0.j],
       [0.+0.j, 0.+0.j, 0.+0.j, 1.+0.j],
       [0.+0.j, 0.+0.j, 1.+0.j, 0.+0.j],
       [0.+0.j, 1.+0.j, 0.+0.j, 0.+0.j]])]
```

```var["name"]``` : gate name list values before are computed.

```Python
[(0, ['I_{q1}', 'X_{q0}']), (1, ['CX_{q0, q1}'])]
```

```var["result"]``` : final result is the calculated matrix.

```Python
array([[0.+0.j, 1.+0.j, 0.+0.j, 0.+0.j],
       [0.+0.j, 0.+0.j, 1.+0.j, 0.+0.j],
       [0.+0.j, 0.+0.j, 0.+0.j, 1.+0.j],
       [1.+0.j, 0.+0.j, 0.+0.j, 0.+0.j]])
```

The raw option creates it as variable that can be used with latex syntax.

```Python
ConversionService(conversion_type="QC_TO_MATRIX", option={"print": "raw"})
```

## quantum circuit to bra-ket

```Python
quantum_circuit = QuantumCircuit(2, 2)
quantum_circuit.h(0)
quantum_circuit.x(0)
quantum_circuit.cx(0, 1)
sample_converter = ConversionService(conversion_type="QC_TO_BRA_KET")
result = sample_converter.convert(input_value=quantum_circuit)
```

You will get the variables : ```result```

```text
(sqrt(2)/2)*|0> + (sqrt(2)/2)*|11>
```

The raw option creates it as variable that can be used with latex syntax.

```Python
ConversionService(conversion_type="QC_TO_BRA_KET", option={"print": "raw"})
```

```tex
\begin{equation}
\\frac{\\sqrt{2}}{2} |00\\rangle+\\frac{\\sqrt{2}}{2} |11\\rangle
\end{equation}
```

You can also add options like below:

```Python
ConversionService(conversion_type="QC_TO_BRA_KET", option={"expression": "simplify"})
```

```Python
ConversionService(conversion_type="QC_TO_BRA_KET", option={"expression": "expand"})
```

## string to bra-ket

```Python
sample_converter = ConversionService(conversion_type="STR_TO_BRA_KET")
result = sample_converter.convert(input_value="sqrt(2)*|00>/2+sqrt(2)*|11>/2")
```

You will get the (sympy) variables : ```result```

```Python
type(result)
```

```text
<class 'sympy.core.add.Add'>
```

The raw option creates it as variable that can be used with latex syntax.

```Python
ConversionService(conversion_type="STR_TO_BRA_KET", option={"print": "raw"})
```

## Options

| convert method | option                                   |
|----------------|------------------------------------------|
| QC_TO_BRA_KET  | expression{simplify, expand}, print{raw} |
| QC_TO_MATRIX   | print{raw}                               |
| MATRIX_TO_QC   | label{str}                               |
| STR_TO_BRA_KET | print{raw}                               |

```python
from qiskit_class_converter import ConversionService

ConversionService(conversion_type="QC_TO_BRA_KET", option={"expression": "simplify"})
```
