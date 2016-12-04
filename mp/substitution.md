#原子替换
　　Pymatgen中用于的原子替换的暂时找到所用函数，暂时找到三个，其大概的作用罗列如下：

* [SubstitutionTransformation](#1-substitutiontransformation)	替换一个元素
* [Substitutor](#2-substitutor)	检查是否能替换
* [MultipleSubstitutionTransformation](#3-multiplesubstitutiontransformation)	多重替换，结构中包含价态

##1. SubstitutionTransformation
　　This transformation substitutes species for one another.
　　**Official links:** [docs](http://pymatgen.org/pymatgen.transformations.html#pymatgen.transformations.standard_transformations.SubstitutionTransformation) [source](http://pymatgen.org/_modules/pymatgen/transformations/standard_transformations.html#SubstitutionTransformation)
>参数:  
**species\_map** – A dict or list of tuples containing the species mapping in string-string pairs. E.g., {“Li”:”Na”} or [(“Fe2+”,”Mn2+”)]. Multiple substitutions can be done. Overloaded to accept sp\_and\_occu dictionary E.g. {“Si: {“Ge”:0.75, “C”:0.25}}, which substitutes a single species with multiple species to generate a disordered structure.    
**apply_transformation(structure)**  
\#被转换结构  
**as\_dict()**  
\#类信息  
**inverse**  
\#当前操作  
**is\_one\_to\_many**  
\#显示是否多值  

```python
#！/bin/usr/env python

from pymatgen.core.structure import Structure
from pymatgen.transformations.standard_transformations import SubstitutionTransformation

ycute2  = Structure.from_file("POSCAR")
lacute2 = SubstitutionTransformation({"Y":"La"}).apply_transformation(ycute2)
```
## 2. Substitutor
**Official links:** [docs](http://pymatgen.org/pymatgen.core.html#pymatgen.core.structure.Structure) [source](http://pymatgen.org/_modules/pymatgen/core/structure.html#Structure)  
class Structure(lattice, species, coords, validate\_proximity=False, to\_unit\_cell=False, coords\_are\_cartesian=False, site\_properties=None)

>参数:  
**threshold** – probability threshold for predictions  
**symprec** – symmetry precision to determine if two structures are duplicates  
**kwargs** – kwargs for the SubstitutionProbability object lambda_table, alpha  
>>**args**  
A tuple of positional arguments values. Dynamically computed from the arguments attribute.  
**kwargs**  
A dict of keyword arguments values. Dynamically computed from the arguments attribute.  

>**as\_dict()**  
\#类信息
classmethod from\_dict()

>**get\_allowed\_species()**
\#该类可以替换的元素
returns the species in the domain of the probability function any other specie will not work

>**pred\_from\_comp(composition)**
Similar to pred\_from\_list except this method returns a list after checking that compositions are charge balanced.

>**pred\_from\_list(species_list)**
There are an exceptionally large number of substitutions to look at (260^n), where n is the number of species in the list. We need a more efficient than brute force way of going through these possibilities. The brute force method would be:

>>```python
output = []
for p in itertools.product(self._sp.species_list
                           , repeat = len(species_list)):
    if self._sp.conditional_probability_list(p, species_list)
                           > self._threshold:
        output.append(dict(zip(species_list,p)))
return output
```
Instead of that we do a branch and bound.
Parameters:

>>**species\_list** – list of species in the starting structure
**Returns**:	list of dictionaries, each including a substitutions dictionary, and a probability value

>**pred\_from\_structures**(target\_species, structures\_list,  remove\_duplicates=True, remove\_existing=False)
performs a structure prediction targeting compounds containing all of the target_species, based on a list of structure (those structures can for instance come from a database like the ICSD). It will return all the structures formed by ionic substitutions with a probability higher than the threshold

>>Notes: If the default probability model is used, input structures must be oxidation state decorated.

>>This method does not change the number of species in a structure. i.e if the number of target species is 3, only input structures containing 3 species will be considered.

>>Parameters:
**target\_species** – a list of species with oxidation states e.g., [Specie(‘Li’,1),Specie(‘Ni’,2), Specie(‘O’,-2)]
**structures_list** – a list of dictionnary of the form {‘structure’:Structure object ,’id’:some id where it comes from} the id can for instance refer to an ICSD id.
**remove\_duplicates** – if True, the duplicates in the predicted structures will be removed
**remove\_existing** – if True, the predicted structures that already exist in the structures\_list will be removed
Returns:
a list of TransformedStructure objects.

```python
from pymatgen import Structure,Composition
from pymatgen.structure_prediction.substitutor import \
Substitutor
bs_struc = Structure.from_file("POSCAR")
bs_comp  = Composition("YCuTe2")
#alw_spec = Substitutor(threshold=0.001, symprec=0.1).\
#get_allowed_species()
#rb_struc = Substitutor(threshold=0.001, symprec=0.1).\
#pred_from_comp(bs_comp)
#rb_struc = Substitutor(threshold=0.001, symprec=0.1).\
#pred_from_structures([('Gd',2)],{'structure':bs_struc},\
True, False)
```
## 3. MultipleSubstitutionTransformation
class MultipleSubstitutionTransformation(sp\_to\_replace, r\_fraction, substitution\_dict, charge\_balance\_species=None, order=True)

>Performs multiple substitutions on a structure. For example, can do a fractional replacement of Ge in LiGePS with a list of species, creating one structure for each substitution. Ordering is done using a dummy element so only one ordering must be done per substitution oxidation state. Charge balancing of the structure is optionally performed.

>Note There are no checks to make sure that removal fractions are possible and rounding may occur. Currently charge balancing only works for removal of species.
Performs multiple fractional substitutions on a transmuter.

>**参数**  
**sp\_to\_replace** – species to be replaced  
\#被替换的元素  
**r\_fraction** – fraction of that specie to replace  
\#替换元素的比例  
**substitution\_dict** – dictionary of the format {2: [“Mg”, “Ti”, “V”, “As”, “Cr”, “Ta”, “N”, “Nb”], 3: [“Ru”, “Fe”, “Co”, “Ce”, “As”, “Cr”, “Ta”, “N”, “Nb”], 4: [“Ru”, “V”, “Cr”, “Ta”, “N”, “Nb”], 5: [“Ru”, “W”, “Mn”] } The number is the charge used for each of the list of elements (an element can be present in multiple lists)  
\#数字代表元素化合价，同一元素可以出现在不同的列表中  
**charge\_balance\_species** – If specified, will balance the charge on the structure using that specie.  
\#是否平衡化合价  
**apply\_transformation**(structure, return\_ranked\_list=False)  
\#应用置换的结构  
**as\_dict()**  
\#类信息  
**inverse**  
\#本次类行为  
**is\_one\_to\_many**  
\#是否返回多值  

```python
#！/bin/usr/env python

from pymatgen import Structure
from pymatgen.transformations.advanced_transformations import\
MultipleSubstitutionTransformation

bs_struc =	Structure.from_file("POSCAR")
rb_struc =	MultipleSubstitutionTransformation("Cu",1,\
				{2:["Mg"]},None,True).\
				apply_transformation(bs_struc,True)
```