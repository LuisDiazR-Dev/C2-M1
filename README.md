# C2-M1
conceptos básicos JavaScript EcmaScript 6


## Declaracion de variables

Existen 3 modos
* var : Sin Scope
* let : Con Scope + sin re declaración
* const : Scope + Imutabilidad*

## Scope

Se le llama Scope a la region de codigo donde se definen las variables y por tanto, donde pueden ser llamadas.
Puede ser global o local.

### Scope Global
Es cuando las variables se declaran fuera de cualquier funsion o bloque de codigo y esta disponible a lo lago de todo el codigo.

´´´js
var variableGlobal = "Soy Global"

function llamarGlobal(){
    console.log (variableGlobal)
}

console.log(variableGlobal + "2") // Soy Global 2

llamarGlobal() // Soy Global

´´´

