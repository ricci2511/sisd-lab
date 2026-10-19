**P1**. Encuentre el comando para generar una matriz 3x3 en la que todos los
elementos valgan “1” y crea esta (llámela A).

Una matrix 3x3 donde todos los elementos toman el valor 1 se puede obtener ejecutando `ones(3, 3)` (el segundo argumento es opcional).

**P2.** Busque en la ayuda interactiva de MATLAB la manera computacionalmente correcta de resolver este sistema.

Sistemas de ecuaciones lineales de la forma $Ax= b$ se pueden resolver eficientemente mediante el comando [`mldivide`](https://es.mathworks.com/help/releases/R2026b/matlab/ref/double.mldivide.html) (o equivalentemente por `A\b`):
```matlab
A=[1 2 3; 0 1 0; 1 0 1];
b=[2 -1 0].';
x = mldivide(A, b) % same as A\b
```

**P3**. ¿Cuántos elementos tiene? Parece que sobra 1, ¿por qué?

La variable `phi = 0:(2*pi)/100:2*pi` contiene 101 elementos porque partimos el intervalo `[0, 2π]` en 100 intervalos, que incluye el 0. Si querríamos que sea de 100 elementos deberíamos dividir por 99.

**P4.** Escriba la secuencia de comandos para representar la señal y=sin(t) tal y como se muestra en la figura 1. Fíjese que tiene que definir tanto el vector t como el vector y para cada representación y superponerlos en la misma gráfica.

```matlab
t = 0:10/100:10;
y = sin(t);
plot(t, y);
hold on % Para superponer la grafica del stem con la del plot
t=0:10;
y = sin(t);
stem(t, y)
```

![[Figure_1.png]]`

**P5**. Defina dos vectores de 4 elementos, ejecute la función y compruebe el resultado. A continuación, modifique la función para que dé un 3er resultado (res3) que consista en la multiplicación elemento a elemento de los dos vectores.

Simplemente añadimos un nuevo resultado que retornamos (`res3`) a la definición de la función y le asignamos la multiplicación elemento a elemento con el operador `.*`.
El archivo `funcionmuysimple.m` editado:
```matlab
function [res1,res2,res3]=funcionmuysimple(par1,par2)

% [res1,res2,res3]=funcionmuysimple(par1,par2)
% Esta funcion asigna a res1 la suma de par1 y par2,
% a res2 la resta y a res3 la multiplicacion elemento
% a elemento

res1=par1+par2;
res2=par1-par2;
res3=par1.*par2;
```

**P6.** ¿Qué variables se han cargado? Nota: la función clear sirve para eliminar
todas las variables del workspace.

Tras ejecutar `load 'remoh1.mat'` se han cargado 2 variables: `fs` y `remoh1`.

**P7**. Escriba la secuencia de comandos de MATLAB para llevar a cabo los
siguientes pasos:

**a.** A partir de los vectores remoh1 y remoh2 cree un vector remoh
alternando muestras de remoh1 y remoh2. Invierta el orden del
vector resultante

```matlab
% Preparamos el vector columna de 18739*2 elementos ya que contendrá los valores de los vectores remoh1 y remoh2
remoh = zeros(length(remoh1)*2, 1)
% Copiamos de 2 en 2 (comenzando en posición 1) el vector remoh1 al vector remoh
remoh(1:2:end) = remoh1
% Copiamos de 2 en 2 (comenzando en posición 2) el vector remoh 2 al vector remoh
remoh(2:2:end) = remoh2
% Inveritmos el orden
remoh = remoh(length(remoh):-1:1)
```

**b.** Guarde el vector en formato wav usando la función audiowrite (use help audiowrite). Fíjese que esta función admite como parámetro una frecuencia de muestreo (fs) que fue cargada al leer 'remoh1.mat'

Ejecutamos `audiowrite('audio.wav', remoh, fs)`, que crea el archivo `audio.wav` a partir del vector `remoh` y la frecuencia de mostreo `fs`.

**P8.**  Reproduzca el fichero generado en Windows. ¿Qué es lo que dice? Escriba las secuencia de comandos de MATLAB para obtener la imagen de la figura 2 que representa el vector remoh. Fíjese que el eje x ha de indicar la duración temporal.

 Escuchamos lo siguiente: "In this house we obey the laws of thermodynamics! - Homer simpson".

Para obtener la figura 2:
```matlab
T = length(remoh)/fs # El periodo (muestras entre mustras por segundo)
t = (0:T/(length(remoh)-1):T)'
```

![[Figure_2.png]]