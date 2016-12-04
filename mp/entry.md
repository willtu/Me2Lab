#Entry parameters

s = Structure.from\_dict(structure['output']['crystal'])  
energy = structure['output']['final\_energy']
parameters = {\  
	"hubbards":structure['hubbards'],\  
	"potcar_spec":structure['input']['potcar_spec']\  
	}  

#Output parameters
elx:['output']['site']['0']['label']