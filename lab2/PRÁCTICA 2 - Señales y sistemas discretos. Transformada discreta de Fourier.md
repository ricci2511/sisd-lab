**P1.** Represente la parte real e imaginaria de una secuencia exponencial
compleja con amplitudes: creciente, decreciente y constante.

Como que nuestra señal se expresa como $x[n]=Ar^ne^{j(wn+\theta)}$, simplemente debemos ajustar $r$. Entonces:
- Si $|r|<1$, obtenemos una exponencial decreciente.
- Si $|r|>1$ obetenemos una exponencial creciente.
- Si $r=1$ obtenemos una exponencial de amplitud constante.

```matlab
n = 0:50;
w = pi/10;
C = 1;

% Amplitud decreciente
r = 0.9;
z = r*exp(j*w); x = C*z.^n;

% Amplitud creciente
r = 1.1;
z = r*exp(j*w); x = C*z.^n;

% Amplitud constante
r = 1;
z = r*exp(j*w); x = C*z.^n;

% Gráfica de parte real e imaginaria (ejecuta tras cada evaluación de z)
subplot(2,1,1); stem(n, real(x)); xlabel('n'); ylabel('Re(x[n])'); subplot(2,1,2); stem(n, imag(x)); xlabel('n'); ylabel('Im(x[n])');
```

**P2**. Represente 32 muestras de dos señales sinusoidales ($x[n]=Asin(wn)$), una que no sea periódica y otra que tenga un periodo de 16 muestras.

Una señal discreta solamente es periódica en $N$ muestras si su frecuencia es un cociente de enteros, es decir, $w=2πf=\frac{2πn}{N} \Rightarrow f=\frac{n}{N}$ donde $n, N\in\mathbb{Z}$.
Entonces una sinusoidal periódica con periodo de 16 muestras se consigue con $w=π/8$ ya que $2π/w=16 \Rightarrow w=π/8$. 

![[sin-periodic-with-16-samples.png]]

Mientras que un ejemplo de una sinusoidal no periódica se puede obtener con $w=\frac{π}{\sqrt{2}}$.

![[sin-non-periodic-32-samples.png]]

**P3.** Sea la secuencia $x[n]=cos(2*pi*3/10*n)$ ¿Cuál es la frecuencia de la sinusoide? Utilizando la función `rep_tyf` obtenga la DFT para secuencias de 10, 31, 50, 83 y 127 muestras. ¿Qué frecuencia diría que se obtiene a partir de la DFT en cada caso? ¿Qué condiciones se tienen que dar en la longitud de la DFT para que el resultado coincida con el esperado?

La frecuencia de la sinusoide es $f=\frac{3}{10}=0.3$.

Frecuencias obtenidas a partir de la DFT para las siguientes muestras:
- 10: $f=0.3$.
- 31: $f_1\approx-0.3064$, $f_2\approx0.2742$.
- 50: $f=0.3$.
- 83: $f_1\approx-0.3072$, $f_2\approx0.2952$.
- 127: $f_1\approx-0.3031$, $f_2\approx0.2953$.

Para que la DFT muestre las frecuencias esperadas, recordemos que $f$ debe ser un cociente de enteros: $\frac{3}{10} = \frac{k}{N} \Rightarrow k=N\frac{3}{10}$. Por tanto la longitud de la DFT ($N$) debe ser un múltiplo de 10, ya que en caso contrario $k\notin \mathbb{Z}$. Por esta razón observamos las frecuencias esperadas con las secuencias de muestras pares (10 y 50), mientras que con las muestras impares (31, 83 y 127) aparecen desplazadas.

**P4.** Busque qué función de MATLAB lleva a cabo la convolución lineal y cuáles
son sus parámetros de entrada y salida. Obtenga la respuesta a la salida de
un sistema promediador de 4 muestras (`h[n] = [0.25 0.25 0.25 0.25]`) cuando
a su entrada se introduce una señal triangular de 7 muestras (`x[n] = [1, 2, 3, 4, 3, 2, 1]`).

La convolución lineal se lleva a cabo con [`conv`](https://es.mathworks.com/help/matlab/ref/conv.html). En este caso dadas las secuencias $h[n]$ y $x[n]$, su salida viene dada por $y[n] = x[n] * h[n]$.

```matlab
h = [0.25 0.25 0.25 0.25];
x = [1, 2, 3, 4, 3, 2, 1];
y = conv(x, h)

% Resultado: |0.2500|0.7500|1.5000|2.5000|3.0000|3.0000|2.5000|1.5000|0.7500|0.2500|
```

**P5.** Obtenga de nuevo la salida del sistema de P4 utilizando el teorema de convolución y la DFT con 8 muestras. Para ello:
**a.** Calcule la DFT de las dos secuencias. Nótese que tendrá que añadir
ceros a cada secuencia para tener 8 muestras.
**b.** Multiplique las secuencias resultantes y obtenga la DFT inversa
(ifft).
**c.** Compare el resultado obtenido en b. con el de la convolución
circular de 8 muestras (cconv) y con P4. ¿Qué diferencias y
similitudes observa en cada caso?

```matlab
% (a): DFT de las dos secuencias de N=8 muestras cada una
N = 8;
h = [0.25 0.25 0.25 0.25];
x = [1, 2, 3, 4, 3, 2, 1];
H_f = fft(h, N)
X_f = fft(x, N)

% (b): Usamos teorema de convolución
Y_f = H_f.*X_f
y = ifft(Y_f)

% (c): Convolución circular de 8 muestras
y_c = cconv(x, h, N)

% Resultado: |1.0000|1.0000|1.5000|2.5000|3.0000|3.0000|2.5000|1.5000|
```

Como es esperado, observamos que el producto de las dos DFTs equivale a la convolución circular en el dominio temporal (Teorema de convolución de la DFT). 
TODO: Compara con `conv`.