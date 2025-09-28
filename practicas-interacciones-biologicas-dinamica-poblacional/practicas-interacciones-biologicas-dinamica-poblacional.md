Práctica 2. Interacciones biológicas con **datos de poblaciones**
================
UASD · Arlen Marmolejo Hernández
2025-09-28

<!-- README.md se genera a partir de README.Rmd. Por favor, edita ese archivo. -->
<style type="text/css">
.exp-box summary{
  display:inline-block; cursor:pointer;
  background:#f5f5f5; border:1px solid #ddd;
  padding:.4em .8em; border-radius:.4em; font-weight:600;
}
.exp-box[open] summary{ background:#e8f5e9; border-color:#c8e6c9; }
.exp-box .content{
  margin:.8em 0 0 0; padding:1em;
  border-left:4px solid #2E8B57; background:#fafafa;
}
.exp-box summary .label-open{ display:none; }
.exp-box[open] summary .label-closed{ display:none; }
.exp-box[open] summary .label-open{ display:inline; }
</style>
<style type="text/css">
 /* Mueve el botón Show/Hide de cada chunk a la izquierda */
.code-folding-btn{
  float: left !important;
  margin: .25rem .6rem .4rem 0; /* separa del código */
}
&#10;/* Asegura que el código empiece debajo del botón */
div.sourceCode, pre {
  clear: both;
}
&#10;/* (Opcional) también mueve el botón global "Show All Code" a la izquierda */
#rmd-show-all-code{
  float: left !important;
  margin-right: .75rem;
}
</style>
<style type="text/css">
/* BOTÓN COPIAR */
/* Contenedor posicionable y con espacio reservado para el botón */
.code-with-copy{
  position: relative;
  --copy-pad: 2.8rem;           /* >= altura del botón */
  padding-bottom: var(--copy-pad);  /* si .code-with-copy es div.sourceCode */
}
&#10;/* Si .code-with-copy está en el PRE, reserva ahí también */
.code-with-copy pre{
  padding-bottom: var(--copy-pad) !important;
  margin-bottom: 0 !important;
}
&#10;/* Variante pandoc: el código está dentro de div.sourceCode > pre */
.code-with-copy div.sourceCode{
  padding-bottom: var(--copy-pad);     /* reserva espacio dentro del scrolleo */
}
.code-with-copy div.sourceCode pre{
  margin-bottom: 0 !important;         /* evita doble espacio visual */
}
&#10;/* Botón "Copiar" abajo-izquierda */
.code-copy-btn{
  position: absolute;
  bottom: .6rem; left: .6rem;   /* antes top/right */
  font-size: .8rem; line-height: 1;
  padding: .35rem .55rem;
  border: 1px solid #d0d7de; border-radius: .35rem;
  background: #f6f8fa; cursor: pointer;
  z-index: 2;
}
.code-copy-btn:hover{ background:#eef2f6; }
&#10;/* (opcional) por si algún tema recorta la altura de línea */
.code-with-copy code{ line-height: 1.25; }
</style>
<script>
window.addEventListener('DOMContentLoaded', function () {
  // Encuentra bloques de código en ambas variantes de Pandoc
  var codeNodes = document.querySelectorAll('div.sourceCode pre code, pre > code');
&#10;  codeNodes.forEach(function(codeEl){
    // Contenedor posicionable: el div.sourceCode si existe, si no el <pre>
    var box = codeEl.closest('div.sourceCode') || codeEl.parentElement;
    if (!box) return;
&#10;    // Evita duplicados si ya existe botón
    if (box.querySelector('.code-copy-btn')) return;
&#10;    // Asegura clase para posicionamiento relativo
    box.classList.add('code-with-copy');
&#10;    // Crea botón
    var btn = document.createElement('button');
    btn.className = 'code-copy-btn';
    btn.type = 'button';
    btn.setAttribute('aria-label', 'Copiar código');
    btn.textContent = 'Copiar';
    box.appendChild(btn);
&#10;    function getCodeText() {
      // innerText preserva saltos de línea visuales
      var txt = codeEl ? codeEl.innerText : '';
      return (txt || '').replace(/\s+$/,'') + '\n';
    }
&#10;    function feedback(ok){
      var old = btn.textContent;
      btn.textContent = ok ? '¡Copiado!' : 'Error';
      btn.disabled = true;
      setTimeout(function(){ btn.textContent = old; btn.disabled = false; }, 1200);
    }
&#10;    function fallbackCopy(text){
      var ta = document.createElement('textarea');
      ta.value = text; ta.style.position = 'fixed'; ta.style.top = '-1000px';
      document.body.appendChild(ta); ta.focus(); ta.select();
      try { feedback(document.execCommand('copy')); }
      catch(e){ feedback(false); }
      document.body.removeChild(ta);
    }
&#10;    btn.addEventListener('click', function(){
      var text = getCodeText();
      if (navigator.clipboard && window.isSecureContext){
        navigator.clipboard.writeText(text).then(function(){ feedback(true); })
          .catch(function(){ fallbackCopy(text); });
      } else {
        fallbackCopy(text);
      }
    });
  });
});
</script>

Versión HTML (quizá más legible),
[aquí](https://ecologia2-master.github.io/practicas/practicas-interacciones-biologicas-dinamica-poblacional/practicas-interacciones-biologicas-dinamica-poblacional.html)

> Donde quiera que veas el botón `Show`/`Hide`, haz clic para
> expandir/colapsar la caja de código.

``` r
# ESTA ES UNA CAJA DE CÓDIGO DE EJEMPLO
```

# 1 Ejercicio 1. **Competencia interespecífica** con el modelo de **Lotka–Volterra**

> Objetivo: evaluar el tipo de competencia LV entre dos especies a
> partir de una tabla pequeña (10 filas) de datos simulados y sus
> parámetros asociados, trabajando **a mano** o con ayuda de una hoja de
> cálculo, y R (opcionalmente).

## 1.1 Marco teórico — **Competencia interespecífica** con el modelo de **Lotka–Volterra**

**Fuente base**: *LibreTexts* — *15.5: Quantifying Competition Using the
Lotka-Volterra Model* (Gettysburg College, Ecology for All). Ver sección
**Referencias** al final.

### 1.1.1 Del **logístico** de una especie a la competencia de **dos especies**

Partimos del crecimiento **logístico** para cada especie en ausencia de
la otra (intraespecífica):

$$
\frac{d N_{1}}{dt}=r_{1} N_{1}\left(\frac{K_{1}-N_{1}}{K_{1}}\right),\qquad
\frac{d N_{2}}{dt}=r_{2} N_{2}\left(\frac{K_{2}-N_{2}}{K_{2}}\right).
$$

$$
\begin{aligned}
t &:\ \text{tiempo},\\[2pt]
N_1,\ N_2 &:\ \text{tamaño poblacional de las especies 1 y 2 (individuos)},\\[2pt]
\frac{dN_1}{dt},\ \frac{dN_2}{dt} &:\ \text{tasas instantáneas de cambio poblacional (individuos/tiempo)},\\[2pt]
r_1,\ r_2 &:\ \text{tasas intrínsecas de crecimiento (tiempo}^{-1}\text{)},\\[2pt]
K_1,\ K_2 &:\ \text{capacidades de carga del ambiente (individuos)},\\[2pt]
\left(\frac{K_1-N_1}{K_1}\right),\ \left(\frac{K_2-N_2}{K_2}\right) 
&:\ \text{factor densodependiente }=1-\frac{N_1}{K_1},\ 1-\frac{N_2}{K_2}.
\end{aligned}
$$

Para incorporar **competencia interespecífica**, suponemos que
individuos de la especie 2 reducen el crecimiento de la 1 en **unidades
equivalentes** a $\alpha_{12}$ individuos de 1 (y viceversa
$\alpha_{21}$). Esto produce las **ecuaciones de Lotka–Volterra
(competencia)**:

$$
\frac{dN_{1}}{dt}= r_{1}N_{1}\!\left(\frac{K_1 - N_{1} - \alpha_{12}N_{2}}{K_{1}}\right),\qquad
\frac{dN_{2}}{dt}= r_{2}N_{2}\!\left(\frac{K_2 - N_{2}-\alpha_{21}N_{1}}{K_{2}}\right).
$$

> $\alpha_{12}$ y $\alpha_{21}$ son **coeficientes de competencia**
> (adimensionales): convierten individuos de una especie en
> “**equivalentes**” de la otra en términos de uso de recursos. También
> se suele usar la notación $\alpha$ para $\alpha_{12}$, y $\beta$ para
> $\alpha_{21}$.

<details class="exp-box">
<summary>

<span class="label-closed">Más información</span>
<span class="label-open">Ocultar</span>

</summary>
<div class="content">

Sobre el modelo logístico, construimos el modelo con competencia. Para
ello, incorporamos la competencia interespecífica en cada una de estas
ecuaciones. Suponemos que cada nuevo integrante de la Población 1 reduce
los recursos disponibles para cada integrante de la Población 2 y, por
lo tanto, disminuye su tasa de crecimiento poblacional. Del mismo modo,
los nuevos integrantes de la Población 2 también reducirán los recursos
disponibles para los miembros de la Población 1; eso es, en esencia, lo
que significa competencia interespecífica.

La forma más simple de modelar esto sería modificar el término
correspondiente al efecto de la densidad. Sin embargo, esa opción supone
que cada individuo adicional de la Población 2 afecta a la Población 1
exactamente igual que lo haría un individuo adicional de la propia
Población 1. Como esto no tiene por qué ser cierto, introducimos un
**coeficiente de competencia** que expresa cuánto influye, en términos
relativos, cada individuo adicional de la Población 2 sobre la Población
1 (comparado con el efecto de un individuo adicional de la Población 1).
El modelo para la Población 2 se modifica de manera paralela. El
resultado es el modelo de Lotka–Volterra para competencia entre dos
especies.

Observa los subíndices de los coeficientes de competencia: el que va “de
2 a 1” expresa el efecto de un miembro de la Población 2 sobre la tasa
de crecimiento de la Población 1; el que va “de 1 a 2” expresa el efecto
de un miembro de la Población 1 sobre la tasa de crecimiento de la
Población 2.

El valor del coeficiente de competencia nos dice algo sobre la
importancia relativa de la competencia **interespecífica** frente a la
**intraespecífica** en la dinámica de una especie:

- Si el coeficiente es **menor que 1**, la competencia
  **intraespecífica** tiene un impacto per cápita más fuerte en la
  disponibilidad de recursos para esa especie.

- Si el coeficiente es **mayor que 1**, la competencia
  **interespecífica** tiene un impacto per cápita más fuerte.

- Si el coeficiente es **igual a 1**, ambos tipos de competencia tienen
  un impacto per cápita similar sobre la disponibilidad de recursos para
  esa especie.

  </div>
  </details>

### 1.1.2 Diagrama de fase, **isoclinas** y **puntos de equilibrio**

Son líneas de crecimiento neto cero (en inglés *zero net growth
isocline*, ZNGI), y se denominan así porque, a lo largo de ellas, el
crecimiento de la población que representan es cero. Las isoclinas se
dibujan sobre un gráfico muy conocido en ecología denominado “**diagrama
de fase**” o “**plano $N_1$–$N_2$**”, en el que se representan los
tamaños de las dos poblaciones de manera conjunta en los ejes $N_1$ y
$N_2$. El diagrama de fase **no incluye el tiempo en sus ejes**, sólo
los tamaños poblacionales.

La isoclina de 1 ($dN_1/dt=0$) cumple $N_{1}+\alpha_{12}N_{2}=K_{1}$.  
La de 2 ($dN_2/dt=0$) cumple $N_{2}+\alpha_{21}N_{1}=K_{2}$.

Sus **interceptos** con los ejes son:

- Para 1: $(K_1,0)$ y $(0,K_1/\alpha_{12})$.
- Para 2: $(0,K_2)$ y $(K_2/\alpha_{21},0)$.

El **cruce** de isoclinas en el diagrama de fase da el equilibrio
interior $(N_1^{*},N_2^{*})$, pero este no siempre existe. Cuando las
isoclinas no se cruzan, el escenario es de exclusión; existe un
escenario donde las isoclinas se intersectan en el que también se
produce exclusión. Para determinar a qué escenario corresponde nuestro
modelo, se evalúan los interceptos en el diagrama de fase:

- **1 excluye 2** si $K_1 > K_2/\alpha_{21}$ **y**
  $K_2 < K_1/\alpha_{12}$.
- **2 excluye 1** si $K_1 < K_2/\alpha_{21}$ **y**
  $K_2 > K_1/\alpha_{12}$.
- **Coexistencia (equilibrio) inestable** si $K_1 > K_2/\alpha_{21}$
  **y** $K_2 > K_1/\alpha_{12}$, donde se pueden presentar condiciones
  también de exclusión en función de las condiciones iniciales.
- **Coexistencia (equilibrio) estable** si $K_1 < K_2/\alpha_{21}$ **y**
  $K_2 < K_1/\alpha_{12}$, y es el único caso donde ambas especies se
  encuentran en equilibrio.

**Lectura visual** del diagrama de fase o plano $N_1$–$N_2$:

- Debajo o a la izquierda de la isoclina de una especie, su población
  **crece** ($dN_i/dt>0$); por encima o la derecha de la isoclina de una
  especie, su población **disminuye**.
- Si las isoclinas no se intersectan, una especie excluye a la otra
  (extinción local de una especie). En concreto, la especie cuya
  isoclina se encuentre más a la derecha-arriba será la especie que
  “vencerá”.
- Si hay intersección de las isoclinas, y las flechas del campo apuntan
  hacia dicha intersección, hay **coexistencia estable**; pero si aún
  habiendo intersección, las flechas del campo “caen” hacia un **eje**,
  se dice que hay **exclusión** (extinción local de una especie).

<details class="exp-box">
<summary>

<span class="label-closed">Más información</span>
<span class="label-open">Ocultar</span>

</summary>
<div class="content">

**Qué son las isoclinas y qué “dicen”**

- **Isoclina de $N_1$**: $N_1 + \alpha_{12}N_2 = K_1$. Debajo/izquierda
  de esa línea se cumple $N_1+\alpha_{12}N_2<K_1$ ⇒ $dN_1/dt>0$ (1
  **aumenta**). Encima/derecha, $dN_1/dt<0$ (1 **disminuye**).
- **Isoclina de $N_2$**: $N_2 + \alpha_{21}N_1 = K_2$. Debajo/izquierda
  de esa línea: $dN_2/dt>0$ (2 **aumenta**). Encima/derecha: $dN_2/dt<0$
  (2 **disminuye**).

Cada isoclina “parte” el diagrama de fase en dos mitades: donde esa
especie crece o decrece.

**Qué significa “converge a la intersección → coexistencia”**

El **único** punto del interior donde **ambas** derivadas son cero a la
vez es el **cruce de isoclinas**. Si las flechas del campo (al combinar
los signos de $dN_1$ y $dN_2$) apuntan **hacia** ese cruce desde los
alrededores, **ese punto es estable**: las dos poblaciones se acercan a
$(N_1^*,N_2^*)$. Eso es **coexistencia estable**.

Regla visual rápida:

- Zona “debajo” de ambas isoclinas: flecha ↗︎ (suben las dos).
- “Debajo” de la de $N_1$ pero “encima” de la de $N_2$: 1 sube, 2 baja →
  flecha →.
- “Encima” de la de $N_1$ pero “debajo” de la de $N_2$: 1 baja, 2 sube →
  flecha ↑.
- “Encima” de ambas: flecha ↙︎ (bajan las dos).

Si esas flechas rodean el cruce y lo “atrapan”, hay coexistencia
estable.

**Qué significa “cae a un eje → exclusión”**

En el diagrama de fase o plano $N_1$-$N_2$, los ejes $N_1=0$ (abajo,
horizontal) y $N_2=0$ (izquierda, vertical) representan **extinción** de
una especie.

- En el eje $N_2=0$: la ecuación de $N_2$ vale 0 para siempre ⇒ si la
  trayectoria llega ahí, **2 queda extinta** y 1 evoluciona sola
  (logístico) hasta $(K_1,0)$.
- En el eje $N_1=0$: análogo, ⇒ la especie **1 queda extinta** y 2
  evoluciona sola hasta alcanzar $(0,K_2)$.

Por eso, cuando una trayectoria el diagrama de fase “**cae a un eje**”,
significa que una especie se **va a 0**, y esto significa **exclusión
competitiva**.

**Analiza el caso “2 excluye a 1” mirando un diagrama de fase** (ver
gráfico superior-derecha de <a href="#casos-canonicos">1.1.2.1.3</a>)

En el diagrama de fase, mira los **interceptos** de las isoclinas en los
ejes:

- En el **eje $N_1$** (abajo): compara $K_1$ (corte de la isoclina de 1)
  con $K_2/\alpha_{21}$ (corte de la isoclina de 2).
- En el **eje $N_2$** (izquierda): compara $K_1/\alpha_{12}$ (corte de
  la isoclina de 1) con $K_2$ (corte de la de 2).

Casos clásicos (geométricos):

- **2 excluye a 1** si la isoclina de 2 queda **más “afuera”** que la de
  1 en **ambos ejes**: $K_1 < K_2/\alpha_{21}$ **y**
  $K_1/\alpha_{12} < K_2$. Intuición: 2 “tolera” más densidad propia y
  del otro (la intra \> inter para 2); las flechas empujan hacia el
  **eje $N_1=0$** ⇒ $N_1\to0$.

- **1 excluye a 2** si pasa lo contrario (isoclina de 1 más afuera en
  ambos ejes).

- **Cruce “alternado”** (uno gana en $x$ y el otro en $y$): hay cruce
  interior.

  - Si **intra \> inter** para ambos (geométricamente: el cruce resulta
    **atractor**), hay **coexistencia estable**.
  - Si **inter \> intra** (el cruce es **inestable**), hay “punto de
    silla” (*saddle point*), separatriz y **efecto de prioridad**: según
    condiciones iniciales, uno u otro excluye.

> Importante: **siempre hay un cruce geométrico** de las líneas con
> parámetros positivos. Lo que cambia no es “si hay cruce”, sino **si
> ese cruce es estable o inestable**. La exclusión **puede ocurrir
> aunque las isoclinas se crucen**, en cuyo caso el cruce es un punto de
> separación o repulsión de las flechas de campo denominado “punto de
> silla” (*saddle point*), y las flechas se van a un eje. También se
> menciona el concepto de separatriz, una línea dibujadada sobre el
> punto de silla, la cual divide el diagrama de fase en dos zonas: si
> las condiciones iniciales están a un lado de la separatriz, una
> especie excluye a la otra; si están al otro lado, ocurre lo contrario.
> En este caso, el resultado final depende de las **condiciones
> iniciales** (efecto de prioridad).

**Cómo leer el diagrama de fase, paso a paso (receta rápida en clase)**

1.  Dibuja los dos cortes en cada eje ($K_1$ vs $K_2/\alpha_{21}$;
    $K_1/\alpha_{12}$ vs $K_2$).
2.  Marca qué regiones son $dN_1/dt>0$ (debajo de su isoclina) y
    $dN_2/dt>0$ (debajo de la suya).
3.  Dibuja 3–4 flechas guía en cada región.
4.  Si las flechas **entran** al cruce ⇒ **coexistencia**. Si las
    flechas **salen** del cruce y terminan en un eje ⇒ **exclusión** (la
    especie del eje opuesto se extingue). `{=html}     </div>`
    `{=html}     </details>`

#### 1.1.2.1 **Diagramas de fase de referencia según las disposición de las isoclinas**

##### 1.1.2.1.1 **Isoclina de la población 1**

<img src="isoclina-cero-n1.png" width="40%" /> <br>

------------------------------------------------------------------------

##### 1.1.2.1.2 **Isoclina de la población 2**

<img src="isoclina-cero-n2.png" width="40%" /> <br>

------------------------------------------------------------------------

##### 1.1.2.1.3 **Los cuatro casos canónicos de competencia LV**

<img src="cuatro-casos-competencia-lv-v2.png" width="80%" /> <br>

------------------------------------------------------------------------

##### 1.1.2.1.4 **Los dos casos de competencia LV con coexistencia: estable vs. inestable**

<img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/lv-panels-1.png" width="80%" />

------------------------------------------------------------------------

### 1.1.3 Trayectorias

**Qué es una trayectoria.** Llamaremos **trayectoria** a la curva
$(N_1(t),N_2(t))$ que sigue el sistema en el plano $N_1$–$N_2$ desde
unas **condiciones iniciales** dadas $(N_1(0),N_2(0))$. Cada punto del
plano lleva asociada una **flecha** (el **campo de vectores**) que
indica la dirección del cambio instantáneo $(\dot N_1,\dot N_2)$; la
trayectoria “sigue” esas flechas.

**Cómo se mueve la trayectoria respecto de las isoclinas.**

- En la **isoclina de $N_1$** (donde $\dot N_1=0$), la componente
  horizontal del movimiento es cero: la trayectoria allí sólo se
  desplaza **verticalmente** (según el signo de $\dot N_2$).
- En la **isoclina de $N_2$** (donde $\dot N_2=0$), la componente
  vertical es cero: la trayectoria allí sólo se desplaza
  **horizontalmente** (según el signo de $\dot N_1$).
- Fuera de las isoclinas, el **signo** de $\dot N_1$ y $\dot N_2$ (según
  qué lado de cada isoclina estés) determina si la flecha apunta ↗︎, ↘︎, ↖︎
  o ↙︎.

**Hacia dónde va una trayectoria.**

- Si las flechas **entran** al cruce de isoclinas, las trayectorias
  **convergen** al equilibrio interior $(N_1^*,N_2^*)$ ⇒ **coexistencia
  estable**.
- Si el cruce es **inestable**, el punto de intersección de las
  isoclinas se denomina “punto de silla” y, sobre éste, se dibuja una
  “**separatriz**” (frontera) que divide los estados iniciales en dos
  **cuencas de atracción**: a un lado, la trayectoria “cae” hacia el eje
  $N_2=0$ $(K_1,0)$ ⇒ **1 excluye 2**; al otro lado, hacia $N_1=0$
  $(0,K_2)$ ⇒ **2 excluye 1**.
- Si la trayectoria llega a un **eje**, la especie correspondiente se
  mantiene en 0 y la otra sigue su **logístico** hasta su $K$.

**Cómo leer una trayectoria (receta rápida).**

1.  Marca los **cuatro interceptos** de las isoclinas (dos por isoclina)
    y dibuja ambas rectas.
2.  Determina el **signo** de $(\dot N_1,\dot N_2)$ en cada región
    (debajo/encima de cada isoclina).
3.  Sitúa tu **punto inicial** y sigue el **campo de flechas**; si tocas
    la isoclina de $N_1$, justo después te moverás **verticalmente**
    hasta tocar la isoclina de $N_2$, y repite; si tocas la de $N_2$, te
    moverás **horizontalmente** hasta tocar la isoclina de $N_1$, y
    repite.
4.  Si te acercas al cruce ⇒ **coexistencia estable**; si te alejas
    hacia un eje ⇒ **coexistencia inestable o exclusión**.

<img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/trayectoria-demo-imp1-1.png" width="70%" />

<br> <br>

<img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/trayectoria-demo-imp2-1.png" width="70%" />

**Qué ver en el gráfico:**

- En **(A)** las trayectorias de distintos puntos iniciales **entran**
  al cruce de isoclinas ⇒ **coexistencia estable**.
- En **(B)** hay dos destinos posibles (hacia $(K_1,0)$ o $(0,K_2)$),
  separados por una **frontera** (separatriz) ⇒ **coexistencia
  inestable** con **efecto de prioridad**: el resultado depende del lado
  en que caiga el estado inicial.

### 1.1.4 **Exclusión competitiva**

> **Definición operativa**: **exclusión competitiva** significa que,
> bajo condiciones constantes, **una especie reduce a la otra hasta
> densidad ≈ 0 en esa comunidad** (*extinción local*). Coloquialmente:
> “**1 saca a 2 del sitio**”. No implica extinción global; la especie
> excluida puede persistir en otro hábitat o volver por inmigración.

En el **modelo de Lotka–Volterra**:

- Decimos **“1 excluye a 2”** cuando la trayectoria termina en
  $(K_1,0)$: la población 2 cae sobre el **eje horizontal, donde
  $N_2=0$** y no se recupera (ver gráfico superior-izquierda en sección
  @ref(#casos-canonicos)).

- Decimos **“2 excluye a 1”** cuando la trayectoria termina en
  $(0,K_2)$: la población 1 cae sobre el **eje vertical, donde $N_1=0$**
  y no se recupera (ver gráfico superior-derecha en sección
  @ref(#casos-canonicos)).

- **Lectura del diagrama de fase o plano $N_1$–$N_2$**:

  - Si las **flechas** del campo llevan hacia el eje horizontal, donde
    $N_2=0$, **2 se extingue localmente, es decir, 1 excluye a 2** (ver
    gráfico superior-izquierda en sección @ref(#casos-canonicos)).
  - Si la flechas llevan al eje vertical, donde $N_1=0$, la excluida es
    **1** (ver gráfico superior-derecha en sección
    @ref(#casos-canonicos)).

- **¿Cuándo ocurre exclusión competitiva?**: cuando la **competencia
  interespecífica** sobre la especie perdedora es tan fuerte que su
  crecimiento es **negativo** frente a la otra, incluso a bajas
  densidades.  

- **¿Cuándo no ocurre exclusión competitiva?**: con **partición de
  nicho**, **variación ambiental**, **heterogeneidad espacial** o
  **rescate por inmigración**, la coexistencia puede mantenerse.

## 1.2 Práctica — **Competencia LV**

> **Importante**: Cada estudiante trabaja el **mismo mandato** pero con
> **datos distintos** y **pequeños** (10 filas) para poder **hacerlo a
> mano**. Se generan a partir de un **pseudónimo** (“Est01”, “Est02”,
> …).

### 1.2.1 Preparación y **pseudónimo**

Copia y pega este código en RStudio y ejecútalo (para ver el código haz
clic en `Show`).

``` r
# Paquetes usados
suppressPackageStartupMessages({
  library(ggplot2)
  library(digest)
})

# Lista ejemplo (puedes sustituir por la de clase)
if (!exists("pseudonimos_clase")){
  pseudonimos_clase <- paste0("Est", sprintf("%02d", 1:30))
}
```

Esta es la lista de pseudónimos.

``` r
cat("Pseudónimos: ", paste(pseudonimos_clase[-1], collapse=", "), "\n")
```

Pseudónimos: Est02, Est03, Est04, Est05, Est06, Est07, Est08, Est09,
Est10, Est11, Est12, Est13, Est14, Est15, Est16, Est17, Est18, Est19,
Est20, Est21, Est22, Est23, Est24, Est25, Est26, Est27, Est28, Est29,
Est30

En el código siguiente, cambia la cadena de caracteres “Est01” por tu
pseudónimo (p. ej. “Est05”). Esto genera tus datos **únicos** y
**reproducibles**. Anuncia tu elección en el foro.

``` r
# Elige tu pseudónimo
MI_PSEUDONIMO <- "Est01"  # <-- CAMBIA AQUÍ
```

### 1.2.2 Generador de datos **reproducibles**

Este es el código que genera la tabla de datos (10 filas) y los
parámetros para trabajar **a mano**. Recuerda que para que este código
produzca tus datos, debes haber ejecutado el bloque anterior con tu
pseudónimo.

``` r
seed_from_name <- function(x){
  # hash a 32 bits reproducible
  hx <- tryCatch(digest::digest(x, algo="xxhash64", serialize=FALSE),
                 error=function(e) digest::digest(x, algo="crc32", serialize=FALSE))
  by <- vapply(seq(1, 8, 2), function(i) strtoi(substr(hx, i, i+1), 16L), numeric(1))
  u32 <- sum(by * c(1,256,65536,16777216))
  as.integer(1 + (u32 %% (.Machine$integer.max - 1)))
}

# Generador competencia LV con tabla pequeña (~10 filas) para trazo a mano
gen_competencia_lv <- function(seed, steps_small=10, steps_traj=300, dt=0.05){
  set.seed(seed + 501)
  K1 <- sample(seq(320, 680, by=20), 1)
  K2 <- sample(seq(280, 640, by=20), 1)
  r1 <- runif(1, 0.35, 0.9)
  r2 <- runif(1, 0.35, 0.9)
  a12 <- runif(1, 0.2, 1.1)
  a21 <- runif(1, 0.2, 1.1)

  # Forzar 4 escenarios variados según semilla
  caso <- (seed %% 4) + 1
  if (caso == 1){ a12 <- max(0.2, a12 * 0.8); a21 <- min(1.1, a21 * 1.2) }      # 1 excluye 2
  if (caso == 2){ a12 <- min(1.1, a12 * 1.2); a21 <- max(0.2, a21 * 0.8) }      # 2 excluye 1
  if (caso == 3){ a12 <- a12 * 0.95; a21 <- a21 * 0.95 }                         # coexistencia estable
  if (caso == 4){ a12 <- a12 * 1.05; a21 <- a21 * 1.05 }                         # coexistencia inestable

  euler <- function(N10,N20,steps,dt){
    N1 <- N2 <- numeric(steps); N1[1] <- N10; N2[1] <- N20
    for (t in 2:steps){
      dN1 <- r1*N1[t-1]*(1 - (N1[t-1] + a12*N2[t-1])/K1)
      dN2 <- r2*N2[t-1]*(1 - (N2[t-1] + a21*N1[t-1])/K2)
      N1[t] <- max(0, N1[t-1] + dN1*dt)
      N2[t] <- max(0, N2[t-1] + dN2*dt)
    }
    data.frame(t = 0:(steps-1)*dt, N1=N1, N2=N2)
  }

  N10 <- round(runif(1, 0.15, 0.85)*K1)
  N20 <- round(runif(1, 0.15, 0.85)*K2)
  traj <- euler(N10, N20, steps_traj, dt)

  # Submuestreo para tabla pequeña (10 puntos) y redondeo
  take <- unique(round(seq(1, nrow(traj), length.out = steps_small)))
  small <- traj[take, c("t","N1","N2")]
  rnd5 <- function(x) 5*round(x/5)
  small$N1 <- rnd5(small$N1); small$N2 <- rnd5(small$N2)

  pars <- c(r1=r1,r2=r2,K1=K1,K2=K2,a12=a12,a21=a21,caso=caso,N10=N10,N20=N20)
  list(data_traj=traj, data_small=small, pars=pars)
}

SEED <- seed_from_name(MI_PSEUDONIMO)
comp <- gen_competencia_lv(SEED)
```

### 1.2.3 **Tabla y parámetros**

``` r
knitr::kable(comp$data_small, row.names = F,
             caption="Competencia LV — tabla de 10 filas para realizar tu ejercicio a mano. El código original produce la tabla del Est01. Recuerda que para que este código produzca tu tabla, debes haber elegido y ejecutado el bloque correspondiente con tu pseudónimo.")
```

|     t |  N1 |  N2 |
|------:|----:|----:|
|  0.00 | 325 | 365 |
|  1.65 | 275 | 355 |
|  3.30 | 255 | 365 |
|  5.00 | 240 | 380 |
|  6.65 | 230 | 390 |
|  8.30 | 225 | 400 |
|  9.95 | 220 | 405 |
| 11.65 | 215 | 415 |
| 13.30 | 210 | 415 |
| 14.95 | 205 | 420 |

<span id="tab:tabla-pequena"></span>Table 1.1: Competencia LV — tabla de
10 filas para realizar tu ejercicio a mano. El código original produce
la tabla del Est01. Recuerda que para que este código produzca tu tabla,
debes haber elegido y ejecutado el bloque correspondiente con tu
pseudónimo.

``` r
knitr::kable(as.data.frame(comp$pars),
             caption="Competencia LV — parámetros. El código original produce la tabla del Est01. Recuerda que para que este código produzca tu tabla, debes haber elegido y ejecutado el bloque correspondiente con tu pseudónimo.",
             col.names=c("Parámetro","Valor"))
```

| Parámetro |       Valor |
|:----------|------------:|
| r1        |   0.7362978 |
| r2        |   0.8315835 |
| K1        | 420.0000000 |
| K2        | 620.0000000 |
| a12       |   0.5178140 |
| a21       |   0.9469748 |
| caso      |   3.0000000 |
| N10       | 327.0000000 |
| N20       | 366.0000000 |

<span id="tab:tabla-pequena"></span>Table 1.1: Competencia LV —
parámetros. El código original produce la tabla del Est01. Recuerda que
para que este código produzca tu tabla, debes haber elegido y ejecutado
el bloque correspondiente con tu pseudónimo.

### 1.2.4 **Desarrolla este ejercicio a mano o con ayuda de una hoja de cálculo con tus datos**

> Ver demostración en la sección
> <a href="#sec-demo-manual-est01">1.2.5</a>

0.  Recuerda que debes **ejecutar los bloques de código anteriores, y
    cambiar por tu pseudónimo en el bloque correspondiente** para que el
    código produzca tus datos.
1.  **Calcula** los **interceptos** de las isoclinas y **anótalos** (1
    decimal):
    - Isoclina de 1: $(K_1,0)$ y $(0,K_1/\alpha_{12})$.
    - Isoclina de 2: $(0,K_2)$ y $(K_2/\alpha_{21},0)$.
2.  **Dibuja a mano** ambas isoclinas en el plano $N_1$–$N_2$ y
    **sombrea** las 4 regiones por signos de $(dN_1/dt,\ dN_2/dt)$:
    `++`, `+-`, `-+`, `--`.
3.  **Coloca** los 10 puntos de tu tabla en el plano $N_1$-$N_2$ (en
    orden de $t$) y **traza** la trayectoria.
4.  **Diagnostica** el resultado (**coexistencia estable / inestable /
    exclusión 1 / exclusión 2**) **argumentando** con los interceptos y
    el sombreado.
5.  **Verifica el signo** de $dN_1/dt$ y $dN_2/dt$ en un punto de **cada
    región**. Elige $(n_1,n_2)$ de referencia y evalúa los signos.

### 1.2.5 **Demostración manual, usando datos de *Est01***

**Parámetros de *Est01***
$r_1=0.7363,\; r_2=0.8316,\; K_1=420,\; K_2=620,\; \alpha_{12}=0.517814,\; \alpha_{21}=0.9469748$

#### 1.2.5.1 **Calcula los interceptos** de las isoclinas (anota a 1 decimal)

Modelo LV de competencia:

$$
\frac{dN_1}{dt}=r_1N_1\!\left(\frac{K_1 - N_1 - \alpha_{12}N_2}{K_1}\right),\qquad
\frac{dN_2}{dt}=r_2N_2\!\left(\frac{K_2 - N_2 - \alpha_{21}N_1}{K_2}\right).
$$

Isoclinas (líneas de crecimiento cero):

- **Isoclina de 1** ($dN_1/dt=0$): $N_1=K_1-\alpha_{12}N_2$ Interceptos:
  $(K_1,0)=(\mathbf{420.0},0)$ y
  $(0,K_1/\alpha_{12})=(0,\mathbf{811.1})$ (cálculo:
  $420/0.517814=811.102\rightarrow \mathbf{811.1}$)
- **Isoclina de 2** ($dN_2/dt=0$): $N_2=K_2-\alpha_{21}N_1$ Interceptos:
  $(0,K_2)=(0,\mathbf{620.0})$ y
  $(K_2/\alpha_{21},0)=(\mathbf{654.7},0)$ (cálculo:
  $620/0.9469748=654.716\rightarrow \mathbf{654.7}$)

**En una hoja de cálculo (opcional):** con celdas `K1=420`, `K2=620`,
`a12=0.517814`, `a21=0.9469748` `=K1/a12` → **811.1**; `=K2/a21` →
**654.7**.

#### 1.2.5.2 **Fija escalas de ejes** para que entren todos los interceptos

Usa límites algo superiores al mayor intercepto de cada eje:

- Horizontal
  $x_{\max}\approx 1.1\times \max(K_1,\;K_2/\alpha_{21})=1.1\times 654.7\approx \mathbf{720}$.
- Vertical
  $y_{\max}\approx 1.1\times \max(K_2,\;K_1/\alpha_{12})=1.1\times 811.1\approx \mathbf{900}$.

Traza ejes $N_1$ (0…720) y $N_2$ (0…900). Rejilla cómoda: pasos de 50.

#### 1.2.5.3 **Dibuja las isoclinas**

Con lápiz, papel y regla, o en una hoja de cálculo usando un gráfico de
dispersión (*XY Scatterplot*), dibuja las isoclinas.

- **Isoclina N1** (**sólida**, p.ej. naranja): une $(\mathbf{420},0)$
  con $(0,\mathbf{811.1})$. Etiqueta: *Isoclina N1 (dN1/dt = 0)*.
- **Isoclina N2** (**punteada**, p.ej. azul): une $(0,\mathbf{620})$ con
  $(\mathbf{654.7},0)$. Etiqueta: *Isoclina N2 (dN2/dt = 0)*.

**En hoja de cálculo:** crea dos series con esos dos puntos por línea →
Gráfico de **Dispersión con líneas**. Formatea N1 **sólida** y N2
**discontinua**.

#### 1.2.5.4 **Sombréa las 4 regiones** por signos de $(dN_1/dt,\ dN_2/dt)$

Regla memotécnica: **debajo** de una isoclina esa especie **crece**;
**encima**, **disminuye**.

- Debajo de ambas → **`++`** (suben 1 y 2).
- Debajo de N1 y **encima** de N2 → **`+-`** (1 sube, 2 baja).
- **Encima** de N1 y debajo de N2 → **`-+`** (1 baja, 2 sube).
- Encima de ambas → **`--`** (bajan 1 y 2).

Sombrea suavemente o escribe los símbolos en cada zona.

#### 1.2.5.5 **Traza tu trayectoria** con la **tabla pequeña** de Est01

Tus 10 puntos $(N_1,N_2)$:

$$
(325,365)\to(275,355)\to(255,365)\to\cdots\to(205,420)
$$

Colócalos en orden de $t$ y **únelos con flechas** para indicar el
sentido temporal.

> Observación de Est01: del primer al segundo punto $N_1\downarrow$ y
> $N_2\downarrow$ (zona `--`), luego $N_1\downarrow$ y $N_2\uparrow$
> (zona `-+`), con flechas **hacia arriba–izquierda** (tendencia al eje
> $N_1=0$).

#### 1.2.5.6 **Verificación del signo** en 4 puntos

Evalúa el paréntesis de cada derivada para las cuatro regiones posibles
del diagrama $N_1$-$N_2$. No necesitas calcular valores exactos de
$dN_1/dt$ y $dN_2/dt$, solo su **signo** (positivo o negativo) en un
punto de referencia en cada región, utilizando como criterio la
comparación del $N_i$ obtenido respecto del $K_i$ de la población:

$$
dN_1/dt\propto\Big(1-\frac{N_1+\alpha_{12}N_2}{K_1}\Big),\qquad
dN_2/dt\propto\Big(1-\frac{N_2+\alpha_{21}N_1}{K_2}\Big).
$$

Ejemplos con *Est01*
($\alpha_{12}=0.517814,\ \alpha_{21}=0.9469748,\ K_1=420,\ K_2=620$):

- **`++`** en $(100,100)$:
  $100+0.5178\cdot100=151.8<420\Rightarrow dN_1>0$;
  $100+0.9470\cdot100=194.7<620\Rightarrow dN_2>0$.
- **`+-`** en $(100,540)$:
  $100+0.5178\cdot540=379.6<420\Rightarrow dN_1>0$;
  $540+0.9470\cdot100=634.7>620\Rightarrow dN_2<0$.
- **`-+`** en $(400,100)$:
  $400+0.5178\cdot100=451.8>420\Rightarrow dN_1<0$;
  $100+0.9470\cdot400=478.8<620\Rightarrow dN_2>0$.
- **`--`** en $(500,600)$:
  $500+0.5178\cdot600=810.7>420\Rightarrow dN_1<0$;
  $600+0.9470\cdot500=1073.5>620\Rightarrow dN_2<0$.

#### 1.2.5.7 **Diagnóstico por interceptos** (casos clásicos) y análisis de la trayectoria con números de Est01

Compara:

- $K_1\stackrel{?}{>}K_2/\alpha_{21}$ → $420\stackrel{?}{>}654.7$ →
  **FALSO**.
- $K_2\stackrel{?}{>}K_1/\alpha_{12}$ → $620\stackrel{?}{>}811.1$ →
  **FALSO**.

Patrón (**FALSO**, **FALSO**) ⇒ **coexistencia estable**

> Con las **condiciones iniciales** de Est01
> $(N_{1,0}\approx327,\ N_{2,0}\approx366)$, los primeros puntos caen en
> `--` y luego en `-+` (flechas hacia **arriba–izquierda**), lo que
> **sugiere** que la trayectoria se dirige hacia el punto de
> intersección o de equilibrio interior $(N_1^*,N_2^*)$. En este
> arreglo, coherente existe “coexistencia estable”: la competencia se
> encuentra en equilibrio y ninguna especie excluye a la otra.

#### 1.2.5.8 Intersección de la isoclinas

Cálculo de la intersección o cruce de isoclinas (equilibrio interior):

$$
N_1^*=\frac{K_1-\alpha_{12}K_2}{1-\alpha_{12}\alpha_{21}}=\mathbf{194.2},\quad
N_2^*=\frac{K_2-\alpha_{21}K_1}{1-\alpha_{12}\alpha_{21}}=\mathbf{436.1}
$$ Coloca este punto de intersección en tu gráfico.

#### 1.2.5.9 **Redacción corta** (modelo de respuesta)

- **Interceptos**: $(420,0),\ (0,811.1),\ (0,620),\ (654.7,0)$.
- **Gráfico**: N1 **sólida**, N2 **punteada**; zonas `++`, `+-`, `-+`,
  `--`; trayectoria con flechas.
- **Diagnóstico**: por desigualdades → **coexistencia estable**.
- **Interpretación ecológica**: bajo condiciones constantes, ambas
  especies persisten en equilibrio; si una especie se reduce, la otra
  crece hasta que la primera se recupera.

#### 1.2.5.10 **Errores comunes** (revísalos antes de entregar)

- Dibujar la **isoclina N1** usando $(0,K_1)$ en vez de
  $(0,K_1/\alpha_{12})$.
- Elegir límites de ejes **demasiado cortos** que **cortan** los
  interceptos.
- No seguir el **orden temporal** al unir los puntos de la trayectoria.
- Concluir “no hay cruce” por mala escala: **las rectas siempre se
  cruzan**; lo que cambia es la **estabilidad**.

### 1.2.6 **Demostración con R usando datos de *Est01*** (puedes aplicarlo a tu caso también)

``` r
pars <- comp$pars
K1 <- pars["K1"]; K2 <- pars["K2"]; a12 <- pars["a12"]; a21 <- pars["a21"]; r1 <- pars["r1"]; r2 <- pars["r2"]

int_N1_x <- K1/a12
int_N1_y <- K1
int_N2_x <- K2/a21
int_N2_y <- K2

cat(sprintf("Interceptos calculados:\n  Isoclina N1: (0, %.1f) y (%.1f, 0)\n  Isoclina N2: (0, %.1f) y (%.1f, 0)\n",
            int_N1_y, int_N1_x, int_N2_y, int_N2_x))
```

Interceptos calculados: Isoclina N1: (0, 420.0) y (811.1, 0) Isoclina
N2: (0, 620.0) y (654.7, 0)

``` r
# Diagnóstico por desigualdades (teórico)
c1 <- (K1 >  K2 / a21) && (K2 < K1 / a12)
c2 <- (K1 <  K2 / a21) && (K2 > K1 / a12)
c3 <- (K1 >  K2 / a21) && (K2 >  K1 / a12)
c4 <- (K1 <  K2 / a21) && (K2 <  K1 / a12)
diag <- if (c1) "1 excluye 2" else if (c2) "2 excluye 1" else if (c3) "coexistencia inestable" else if (c4) "coexistencia estable" else "indeterminado"
cat("Diagnóstico teórico por interceptos:", diag, "\n")
```

Diagnóstico teórico por interceptos: coexistencia estable

### 1.2.7 Gráfico interpretativo (**sombras + flechas + trayectoria**)

``` r
# --- Interceptos correctos y límites adecuados ---

# Interceptos por isoclina
# N1-iso: N1 = K1 - a12*N2  ->  (x = K1, y = 0) y (x = 0, y = K1/a12)
xint_N1iso <- K1
yint_N1iso <- K1 / a12

# N2-iso: N2 = K2 - a21*N1 ->  (x = K2/a21, y = 0) y (x = 0, y = K2)
xint_N2iso <- K2 / a21
yint_N2iso <- K2

# Límites que siempre muestren TODOS los interceptos
x_max <- 1.08 * max(xint_N1iso, xint_N2iso, na.rm = TRUE)
y_max <- 1.08 * max(yint_N1iso, yint_N2iso, na.rm = TRUE)

# Malla para sombreado de regiones por signos
nx <- 160; ny <- 160
grid <- expand.grid(N1 = seq(0, x_max, length.out = nx),
                    N2 = seq(0, y_max, length.out = ny))
grid$dN1 <- r1*grid$N1*(1 - (grid$N1 + a12*grid$N2)/K1)
grid$dN2 <- r2*grid$N2*(1 - (grid$N2 + a21*grid$N1)/K2)
grid$region <- with(grid,
  ifelse(dN1>0 & dN2>0, "++ (suben 1 y 2)",
  ifelse(dN1>0 & dN2<0, "+- (1 sube, 2 baja)",
  ifelse(dN1<0 & dN2>0, "-+ (1 baja, 2 sube)",
                     "-- (bajan 1 y 2)"))))
grid$region <- factor(grid$region,
  levels = c("++ (suben 1 y 2)","+- (1 sube, 2 baja)",
             "-+ (1 baja, 2 sube)","-- (bajan 1 y 2)"))

# Campo de vectores (flechas)
gd <- expand.grid(N1 = seq(0, x_max, length.out = 16),
                  N2 = seq(0, y_max, length.out = 16))
gd$dN1 <- r1*gd$N1*(1 - (gd$N1 + a12*gd$N2)/K1)
gd$dN2 <- r2*gd$N2*(1 - (gd$N2 + a21*gd$N1)/K2)
L <- sqrt(gd$dN1^2 + gd$dN2^2); L[L==0] <- 1e-9
gd$u1 <- gd$dN1/L; gd$u2 <- gd$dN2/L
arrow_len <- 0.07 * max(x_max, y_max)
gd$xend <- gd$N1 + arrow_len*gd$u1
gd$yend <- gd$N2 + arrow_len*gd$u2

# Isoclinas trazadas SOLO entre sus interceptos
# N2-iso: variar N1 de 0 a xint_N2iso
n1_seg <- seq(0, xint_N2iso, length.out = 500)
n2_iso <- K2 - a21*n1_seg

# N1-iso: variar N2 de 0 a yint_N1iso
n2_seg <- seq(0, yint_N1iso, length.out = 500)
n1_iso <- K1 - a12*n2_seg

df_iso <- rbind(
  data.frame(N1 = n1_iso, N2 = n2_seg,
             iso = "Isoclina N1 (dN1/dt = 0)"),
  data.frame(N1 = n1_seg, N2 = n2_iso,
             iso = "Isoclina N2 (dN2/dt = 0)")
)

# Puntos y etiquetas de interceptos (en el eje correcto)
pts_iso <- data.frame(
  N1  = c(xint_N1iso, 0,            0,           xint_N2iso),
  N2  = c(0,          yint_N1iso,   yint_N2iso,  0),
  lab = c("K[1]", "K[1]/alpha[12]", "K[2]", "K[2]/alpha[21]")
)

# Gráfico
ggplot() +
  geom_raster(data = grid, aes(N1, N2, fill = region),
              alpha = 0.35, interpolate = TRUE) +
  scale_fill_manual(values = c("++ (suben 1 y 2)" = "#4CAF50",
                               "+- (1 sube, 2 baja)" = "#29B6F6",
                               "-+ (1 baja, 2 sube)" = "#FFB74D",
                               "-- (bajan 1 y 2)" = "#BA68C8"),
                    name = "Regiones") +
  geom_segment(data = gd, aes(x = N1, y = N2, xend = xend, yend = yend),
               arrow = arrow(length = grid::unit(3,"pt")),
               linewidth = 0.3, alpha = 0.8) +
  geom_path(data = df_iso, aes(N1, N2, color = iso, linetype = iso),
            linewidth = 1.2) +
  scale_color_manual(values = c("Isoclina N1 (dN1/dt = 0)" = "#E69138",  # naranja sólida
                                "Isoclina N2 (dN2/dt = 0)" = "#3C78D8"), # azul punteada
                     name = NULL) +
  scale_linetype_manual(values = c("Isoclina N1 (dN1/dt = 0)" = "solid",
                                   "Isoclina N2 (dN2/dt = 0)" = "22"),
                        name = NULL) +
  geom_point(data = pts_iso, aes(N1, N2), size = 2.2) +
  geom_text(data = pts_iso, aes(N1, N2, label = lab), parse = TRUE,
            nudge_x = 0.02*max(x_max,y_max),
            nudge_y = 0.02*max(x_max,y_max),
            size = 3.3) +
  geom_point(data = comp$data_small, aes(N1, N2), color = "#2E8B57", size = 2) +
  geom_path (data = comp$data_traj,  aes(N1, N2), color = "#2E8B57",
             linewidth = 1.0, alpha = 0.85) +
  coord_cartesian(xlim = c(0, x_max), ylim = c(0, y_max), expand = TRUE) +
  labs(title = "Competencia LV — isoclinas (convención), regiones (sombras), \ncampo (flechas) y trayectoria (puntos y línea verde)",
       x = "N1", y = "N2") +
  theme_minimal() +
  theme(legend.position = "right",
        legend.box.margin = margin(t = 2, r = 2, b = 2, l = 2))
```

<img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/grafico-interpretativo1-1.png" width="100%" />

> **Refrescando las figuras de referencia sobre isoclinas**

![](cuatro-casos-competencia-lv-v2.png)

## 1.3 Todos los gráficos

``` r
invisible(sapply(pseudonimos_clase, function(x) {
  SEED <- seed_from_name(x)
  comp <- gen_competencia_lv(SEED); 
  pars <- comp$pars
  K1 <- pars["K1"]; K2 <- pars["K2"]; a12 <- pars["a12"]; a21 <- pars["a21"]; r1 <- pars["r1"]; r2 <- pars["r2"]
  int_N1_x <- K1/a12
  int_N1_y <- K1
  int_N2_x <- K2/a21
  int_N2_y <- K2
  # Interceptos por isoclina
  # N1-iso: N1 = K1 - a12*N2  ->  (x = K1, y = 0) y (x = 0, y = K1/a12)
  xint_N1iso <- K1
  yint_N1iso <- K1 / a12
  
  # N2-iso: N2 = K2 - a21*N1 ->  (x = K2/a21, y = 0) y (x = 0, y = K2)
  xint_N2iso <- K2 / a21
  yint_N2iso <- K2
  
  # Límites que siempre muestren TODOS los interceptos
  x_max <- 1.08 * max(xint_N1iso, xint_N2iso, na.rm = TRUE)
  y_max <- 1.08 * max(yint_N1iso, yint_N2iso, na.rm = TRUE)
  
  # Malla para sombreado de regiones por signos
  nx <- 160; ny <- 160
  grid <- expand.grid(N1 = seq(0, x_max, length.out = nx),
                      N2 = seq(0, y_max, length.out = ny))
  grid$dN1 <- r1*grid$N1*(1 - (grid$N1 + a12*grid$N2)/K1)
  grid$dN2 <- r2*grid$N2*(1 - (grid$N2 + a21*grid$N1)/K2)
  grid$region <- with(grid,
                      ifelse(dN1>0 & dN2>0, "++ (suben 1 y 2)",
                             ifelse(dN1>0 & dN2<0, "+- (1 sube, 2 baja)",
                                    ifelse(dN1<0 & dN2>0, "-+ (1 baja, 2 sube)",
                                           "-- (bajan 1 y 2)"))))
  grid$region <- factor(grid$region,
                        levels = c("++ (suben 1 y 2)","+- (1 sube, 2 baja)",
                                   "-+ (1 baja, 2 sube)","-- (bajan 1 y 2)"))
  
  # Campo de vectores (flechas)
  gd <- expand.grid(N1 = seq(0, x_max, length.out = 16),
                    N2 = seq(0, y_max, length.out = 16))
  gd$dN1 <- r1*gd$N1*(1 - (gd$N1 + a12*gd$N2)/K1)
  gd$dN2 <- r2*gd$N2*(1 - (gd$N2 + a21*gd$N1)/K2)
  L <- sqrt(gd$dN1^2 + gd$dN2^2); L[L==0] <- 1e-9
  gd$u1 <- gd$dN1/L; gd$u2 <- gd$dN2/L
  arrow_len <- 0.07 * max(x_max, y_max)
  gd$xend <- gd$N1 + arrow_len*gd$u1
  gd$yend <- gd$N2 + arrow_len*gd$u2
  
  # Isoclinas trazadas SOLO entre sus interceptos
  # N2-iso: variar N1 de 0 a xint_N2iso
  n1_seg <- seq(0, xint_N2iso, length.out = 500)
  n2_iso <- K2 - a21*n1_seg
  
  # N1-iso: variar N2 de 0 a yint_N1iso
  n2_seg <- seq(0, yint_N1iso, length.out = 500)
  n1_iso <- K1 - a12*n2_seg
  
  df_iso <- rbind(
    data.frame(N1 = n1_iso, N2 = n2_seg,
               iso = "Isoclina N1 (dN1/dt = 0)"),
    data.frame(N1 = n1_seg, N2 = n2_iso,
               iso = "Isoclina N2 (dN2/dt = 0)")
  )
  
  # Puntos y etiquetas de interceptos (en el eje correcto)
  pts_iso <- data.frame(
    N1  = c(xint_N1iso, 0,            0,           xint_N2iso),
    N2  = c(0,          yint_N1iso,   yint_N2iso,  0),
    lab = c("K[1]", "K[1]/alpha[12]", "K[2]", "K[2]/alpha[21]")
  )
  
  # Gráfico
  p <- ggplot() +
    geom_raster(data = grid, aes(N1, N2, fill = region),
                alpha = 0.35, interpolate = TRUE) +
    scale_fill_manual(values = c("++ (suben 1 y 2)" = "#4CAF50",
                                 "+- (1 sube, 2 baja)" = "#29B6F6",
                                 "-+ (1 baja, 2 sube)" = "#FFB74D",
                                 "-- (bajan 1 y 2)" = "#BA68C8"),
                      name = "Regiones") +
    geom_segment(data = gd, aes(x = N1, y = N2, xend = xend, yend = yend),
                 arrow = arrow(length = grid::unit(3,"pt")),
                 linewidth = 0.3, alpha = 0.8) +
    geom_path(data = df_iso, aes(N1, N2, color = iso, linetype = iso),
              linewidth = 1.2) +
    scale_color_manual(values = c("Isoclina N1 (dN1/dt = 0)" = "#E69138",  # naranja sólida
                                  "Isoclina N2 (dN2/dt = 0)" = "#3C78D8"), # azul punteada
                       name = NULL) +
    scale_linetype_manual(values = c("Isoclina N1 (dN1/dt = 0)" = "solid",
                                     "Isoclina N2 (dN2/dt = 0)" = "22"),
                          name = NULL) +
    geom_point(data = pts_iso, aes(N1, N2), size = 2.2) +
    geom_text(data = pts_iso, aes(N1, N2, label = lab), parse = TRUE,
              nudge_x = 0.02*max(x_max,y_max),
              nudge_y = 0.02*max(x_max,y_max),
              size = 3.3) +
    geom_point(data = comp$data_small, aes(N1, N2), color = "#2E8B57", size = 2) +
    geom_path (data = comp$data_traj,  aes(N1, N2), color = "#2E8B57",
               linewidth = 1.0, alpha = 0.85) +
    coord_cartesian(xlim = c(0, x_max), ylim = c(0, y_max), expand = TRUE) +
    labs(title = paste("Competencia LV — isoclinas (convención), regiones (sombras),\n",
                       "campo (flechas) y trayectoria (puntos y línea verde)\n", x),
         x = "N1", y = "N2") +
    theme_minimal() +
    theme(legend.position = "right",
          legend.box.margin = margin(t = 2, r = 2, b = 2, l = 2))
  print(p)}, USE.NAMES = T, simplify = F))
```

<img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/unnamed-chunk-11-1.png" width="100%" /><img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/unnamed-chunk-11-2.png" width="100%" /><img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/unnamed-chunk-11-3.png" width="100%" /><img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/unnamed-chunk-11-4.png" width="100%" /><img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/unnamed-chunk-11-5.png" width="100%" /><img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/unnamed-chunk-11-6.png" width="100%" /><img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/unnamed-chunk-11-7.png" width="100%" /><img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/unnamed-chunk-11-8.png" width="100%" /><img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/unnamed-chunk-11-9.png" width="100%" /><img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/unnamed-chunk-11-10.png" width="100%" /><img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/unnamed-chunk-11-11.png" width="100%" /><img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/unnamed-chunk-11-12.png" width="100%" /><img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/unnamed-chunk-11-13.png" width="100%" /><img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/unnamed-chunk-11-14.png" width="100%" /><img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/unnamed-chunk-11-15.png" width="100%" /><img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/unnamed-chunk-11-16.png" width="100%" /><img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/unnamed-chunk-11-17.png" width="100%" /><img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/unnamed-chunk-11-18.png" width="100%" /><img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/unnamed-chunk-11-19.png" width="100%" /><img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/unnamed-chunk-11-20.png" width="100%" /><img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/unnamed-chunk-11-21.png" width="100%" /><img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/unnamed-chunk-11-22.png" width="100%" /><img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/unnamed-chunk-11-23.png" width="100%" /><img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/unnamed-chunk-11-24.png" width="100%" /><img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/unnamed-chunk-11-25.png" width="100%" /><img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/unnamed-chunk-11-26.png" width="100%" /><img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/unnamed-chunk-11-27.png" width="100%" /><img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/unnamed-chunk-11-28.png" width="100%" /><img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/unnamed-chunk-11-29.png" width="100%" /><img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/unnamed-chunk-11-30.png" width="100%" />

## 1.4 Jugando con el paquete `ecostudy`

Te dejo este código por aquí, por si quieres profundizar con este
paquete de R en el modelo de competencia de Lotka–Volterra. No es
obligatorio que lo uses, sólo para quien quiera experimentar más.

``` r
# devtools::install_github("jarioksa/ecostudy")
library(ecostudy)
{
  mod <- lotkacomp(0.8, 0.6, 20, 20)
  plot(mod)
  ## Add trajectory line and starting point
  lines(mod, 1, 1, col="green", lwd=2)
  points(1, 1, pch=16, col="green")
  ## Plot populations against time
  plot(traj(mod, 1, 1))
  ## Textual output
  mod
  summary(mod)
}
```

<img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/unnamed-chunk-12-1.png" width="60%" /><img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/unnamed-chunk-12-2.png" width="60%" />

    ## 
    ## Lotka-Volterra competition model
    ## Summary: stable equilibrium 
    ## Stable solution:
    ##         species 1 species 2
    ## outcome     7.692     15.38

``` r
mod <- lotkacomp(0.8, 0.6, 20, 20); plot(mod, arrows = 10)
```

<img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/unnamed-chunk-12-3.png" width="60%" />

``` r
mod <- lotkacomp(1.8, 1.6, 20, 20); plot(mod, arrows = 10)
```

<img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/unnamed-chunk-12-4.png" width="60%" />

``` r
mod <- lotkacomp(1.8, 0.6, 20, 20); plot(mod, arrows = 10)
```

<img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/unnamed-chunk-12-5.png" width="60%" />

``` r
mod <- lotkacomp(0.9, 0.3, 20, 20); plot(mod, arrows = 10)
```

<img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/unnamed-chunk-12-6.png" width="60%" />

``` r
mod <- lotkacomp(0.9, 0.3, 10, 20); plot(mod, arrows = 10)
```

<img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/unnamed-chunk-12-7.png" width="60%" />

``` r
mod <- lotkacomp(0.5, 0.9, 20, 10); plot(mod, arrows = 10)
```

<img src="practicas-interacciones-biologicas-dinamica-poblacional_files/figure-gfm/unnamed-chunk-12-8.png" width="60%" />

------------------------------------------------------------------------

<!-- # Ejercicio 2. **Depredador–presa (LV)** — *con tus datos* -->
<!-- ## Datos -->
<!-- ```{r} -->
<!-- # Depredador–presa LV: presa (N) y depredador (P) -->
<!-- gen_predpresa_lv <- function(seed, tmax=150, by=0.2){ -->
<!--   set.seed(seed + 602) -->
<!--   r <- runif(1, 0.3, 1.1) -->
<!--   a <- runif(1, 0.006, 0.02) -->
<!--   b <- runif(1, 0.06, 0.2) -->
<!--   m <- runif(1, 0.25, 0.9) -->
<!--   N0 <- sample(20:80, 1) -->
<!--   P0 <- sample(6:20, 1) -->
<!--   pars <- c(r=r, a=a, b=b, m=m) -->
<!--   tt <- seq(0, tmax, by=by) -->
<!--   lv <- function(t, y, p){ -->
<!--     N <- y[1]; P <- y[2] -->
<!--     with(as.list(p), { -->
<!--       dN <- r*N - a*N*P -->
<!--       dP <- b*a*N*P - m*P -->
<!--       list(c(dN, dP)) -->
<!--     }) -->
<!--   } -->
<!--   out <- as.data.frame(ode(y=c(N=N0, P=P0), times=tt, func=lv, parms=pars)) -->
<!--   pars2 <- c(pars, N0=N0, P0=P0, N_star=m/(b*a), P_star=r/a) -->
<!--   list(data=out, pars=pars2) -->
<!-- } -->
<!-- # Construye datos para MI_PSEUDONIMO -->
<!-- pp   <- gen_predpresa_lv(SEED) -->
<!-- ``` -->
<!-- ## **A mano** (entregable) -->
<!-- 1. Con tus parámetros $r,a,b,m$ calcula $N^*=\frac{m}{ba}$, $P^*=\frac{r}{a}$.   -->
<!-- 2. Dibuja las **nullclines** (líneas de crecimiento cero) en el plano $N$–$P$.   -->
<!-- 3. Señala la dirección cualitativa del campo de vectores en 4 regiones. -->
<!-- ## **En R**: series y plano de fase -->
<!-- ```{r pp-R, fig.width=7, fig.height=5} -->
<!-- pp_dat <- pp$data; pp_par <- pp$pars -->
<!-- Nstar <- pp_par["N_star"]; Pstar <- pp_par["P_star"] -->
<!-- par(mfrow=c(1,2)) -->
<!-- plot(pp_dat$time, pp_dat$N, type="l", xlab="t", ylab="N (presa)", main="Serie temporal (presa)") -->
<!-- plot(pp_dat$N, pp_dat$P, type="l", xlab="N", ylab="P", main="Plano de fase") -->
<!-- abline(v = Nstar, h = Pstar, lty=2) -->
<!-- par(mfrow=c(1,1)) -->
<!-- pp_par[c("r","a","b","m","N_star","P_star")] -->
<!-- ``` -->
<!-- **Interpretación**: cómo cambian amplitudes/períodos cuando aumentan/disminuyen $a,b,m$; relación de fase (pico de $N$ precede a $P$). -->

# 2 Referencias

- *LibreTexts*: **15.5: Quantifying Competition Using the Lotka–Volterra
  Model** (Gettysburg College, *Ecology for All*). Licencia **CC
  BY-NC-SA**. URL:
  <https://bio.libretexts.org/Courses/Gettysburg_College/01%3A_Ecology_for_All/15%3A_Competition/15.05%3A_Quantifying_Competition_Using_the_Lotka-Volterra_Model>
