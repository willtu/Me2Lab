#从pymatgen中获取特定化合价元素的比较

##1. 筛选元素函数

　　参照 pymatgen.core.periodic\_table中，Element class内的print\_periodic\_table [docs](http://pymatgen.org/pymatgen.core.html#pymatgen.core.periodic_table.Element.print_periodic_table) [source](http://pymatgen.org/_modules/pymatgen/core/periodic_table.html#Element.print_periodic_table) 编写了元素筛选，输出为list的函数，具体代码见下：  
　　Element_plus.py

```python
from pymatgen.core.periodic_table import Element

def list_filter_elements(filter_function=None):

    """
    A pretty ASCII printer for the periodic table, based on some
    filter_function.

    Args:
        filter_function: A filtering function taking an Element as input
            and returning a boolean. For example, setting
            filter_function = lambda el: el.X > 2 will print a periodic
            table containing only elements with electronegativity > 2.
    """

    filter_els = []
    for atomic_no in range(1, 103):
        try:
            el = Element.from_Z(atomic_no)
        except ValueError:
            el = None
        if el and ((not filter_function) or filter_function(el)):
            filter_els.append("{:s}".format(el.symbol))
    return filter_els
```

##2. common\_oxidation\_states与oxidation\_states的比较

　　pymatgen的Element Class中有两种元素化合价，一种是化合价(oxidation\_states)，另一种是常规化合价(common\_oxidation\_states)，用两者获取+3，+2，+1，-2价元素区别如下：  
**Code:**

```python
from Element_plus import list_filter_elements
# oxidation_states
list_filter_elements(lambda el:化合价 in el.oxidation_states)
# common_oxidation_states
list_filter_elements(lambda el:化合价 in el.common_oxidation_states)
```
**Output:**  

|价态|化合价(oxidation\_states)|常规化合价(common\_oxidation\_states)|
|---|---|---|
|+3|['B', 'C', 'N', 'Al', 'Si', 'P', 'S', 'Cl', 'Sc', 'Ti', 'V', 'Cr', 'Mn', 'Fe', 'Co', 'Ni', 'Cu', 'Ga', 'Ge', 'As', 'Br', 'Y', 'Zr', 'Nb', 'Mo', 'Tc', 'Ru', 'Rh', 'Ag', 'In', 'Sb', 'I', 'La', 'Ce', 'Pr', 'Nd', 'Pm', 'Sm', 'Eu', 'Gd', 'Tb', 'Dy', 'Ho', 'Er', 'Tm', 'Yb', 'Lu', 'Hf', 'Ta', 'W', 'Re', 'Os', 'Ir', 'Au', 'Tl', 'Bi', 'At', 'Ac', 'Th', 'Pa', 'U', 'Np', 'Pu', 'Am', 'Cm', 'Bk', 'Cf', 'Es', 'Fm', 'Md', 'No']|['B', 'N', 'Al', 'P', 'Cl', 'Sc', 'Cr', 'Fe', 'Co', 'Ga', 'As', 'Br', 'Y', 'Ru', 'Rh', 'In', 'Sb', 'I', 'La', 'Ce', 'Pr', 'Nd', 'Pm', 'Sm', 'Eu', 'Gd', 'Tb', 'Dy', 'Ho', 'Er', 'Tm', 'Yb', 'Lu', 'Ir', 'Au', 'Tl', 'Bi', 'Ac', 'Am', 'Cm', 'Bk', 'Cf', 'Es', 'Fm', 'Md', 'No']|
|+2|['Be', 'B', 'C', 'N', 'O', 'Mg', 'Si', 'P', 'S', 'Cl', 'Ca', 'Sc', 'Ti', 'V', 'Cr', 'Mn', 'Fe', 'Co', 'Ni', 'Cu', 'Zn', 'Ga', 'Ge', 'As', 'Se', 'Sr', 'Y', 'Zr', 'Nb', 'Mo', 'Tc', 'Ru', 'Rh', 'Pd', 'Ag', 'Cd', 'In', 'Sn', 'Te', 'Ba', 'La', 'Ce', 'Pr', 'Nd', 'Sm', 'Eu', 'Gd', 'Dy', 'Tm', 'Yb', 'Hf', 'Ta', 'W', 'Re', 'Os', 'Ir', 'Pt', 'Au', 'Hg', 'Pb', 'Po', 'Ra', 'Th', 'Am', 'Cf', 'Es', 'Fm', 'Md', 'No']|['Be', 'Mg', 'S', 'Ca', 'Mn', 'Fe', 'Co', 'Ni', 'Cu', 'Zn', 'Ge', 'Se', 'Sr', 'Pd', 'Cd', 'Sn', 'Te', 'Ba', 'Eu', 'Pt', 'Hg', 'Pb', 'Po', 'Ra']|
|+1|['H', 'Li', 'B', 'C', 'N', 'O', 'Na', 'Mg', 'Al', 'Si', 'P', 'S', 'Cl', 'K', 'Sc', 'V', 'Cr', 'Mn', 'Fe', 'Co', 'Ni', 'Cu', 'Zn', 'Ga', 'Ge', 'Br', 'Rb', 'Y', 'Zr', 'Mo', 'Tc', 'Ru', 'Rh', 'Ag', 'Cd', 'In', 'I', 'Cs', 'Gd', 'Tb', 'W', 'Re', 'Os', 'Ir', 'Au', 'Hg', 'Tl', 'At', 'Fr']|['H', 'Li', 'Na', 'Cl', 'K', 'Br', 'Rb', 'Ag', 'I', 'Cs', 'Hg', 'Tl', 'At', 'Fr']|
|-2|['C', 'N', 'O', 'Si', 'P', 'S', 'Cr', 'Mn', 'Fe', 'Se', 'Mo', 'Ru', 'Te', 'W', 'Os', 'Pt', 'Po']|['O', 'S', 'Se', 'Te', 'Po']|