---
{"dg-publish":true,"permalink":"/ai/ai/uni/06-seminar-svm-kernel/"}
---

# Exercițiul 1 

Fie mulțimea de exemple de antrenare 

$$
S = \left\{ 
\begin{align}
 & x_{1} =  ([-2, 1, 3, 1, -3, 1], 1),  \\
 & x_{2} = ([3, 1, -3, -1, 4, 0], -1),  \\
 & x_{3} =  ([0, 3, -5, -1, 1, 0], 1),  \\
\end{align}
\right\}
$$
Și exemplul de testare 

$$
t = [0, 1, -2, -4, 2, 0]
$$
a) Matricea kernel pentru datele de antrenare 

$$
K_{ij} = k(x_{i}, x_{j}) = \sum_{l} \min\{x_{il}, x_{jl}\}
$$
$$
\begin{align}
 & k(x_{1}, x_{1}) = -2 + 1 + 3 + 1 -3 + 1 = 1 \\
 & k(x_{1}, x_{2}) = -2 + 1 -3 -1 -3 + 0 = -8 \\
 & k(x_{1}, x_{3}) = -2 + 1 -5 -1 -3 + 0 = -10  \\
 & k(x_{2}, x_{2}) = 3 + 1 -3 -1 + 4 + 0 = 4  \\
 & k(x_{2}, x_{3}) = 0 + 1 -5 -1 + 1 + 0 = -4  \\
 & k(x_{3}, x_{3}) = 0 +3 -5 -1 + 1 + 0 = -2
\end{align}
$$

Deci matricea Kernel va fi, pentru datele de antrenare

$$
\mathbf{K} = \begin{pmatrix}
1 & -8 & -10  \\
-8 & 4 & -4 \\
-10 & -4 & -2
\end{pmatrix}
$$
Iar pentru datele de test vim avea 

$$
\mathbf{K}_{t} = \begin{pmatrix}
 k(t, x_{1})  &  
k(t, x_{2})  &  
k(t, x_{3})
\end{pmatrix}
$$
$$
\begin{align}
 & k(t, x_{1}) = -2 + 1 - 2 -4 - 3 + 0 = -10 \\
 & k(t, x_{2}) = 0 + 1 - 3 -4 + 2 + 0 = -4  \\
 & k(t, x_{3}) = 0 + 1 - 5 -4 + 1 + 0 = - 7
\end{align}
$$
Deci 
$$
\mathbf{K}_{t} = (-10, -4, -7)
$$

Normalizarea unei matrici este 

$$
\hat{K}_{ij} =  \frac{K_{ij}}{\sqrt{ K_{ii} \cdot K_{jj} }}
$$
Deci

$$
\mathbf{\hat{K}} = \begin{pmatrix}
1 & -4 & \frac{-10}{\sqrt{ -2 }}  \\
-4 & 1 & \frac{-2}{\sqrt{ -2 }} \\
\frac{-10}{\sqrt{ -2 }}  & \frac{-2}{\sqrt{ -2 }} & -1
\end{pmatrix}
$$
Pentru test, la normalizarea avem nevoie de 
$$
k(t, t) = 0 + 1 -2 -4 + 2 + 0 = -3
$$
Normalizarea va fi 

$$
\hat{k}(t, x_{i}) = \frac{k(t, x_{i})}{
\sqrt{ k(t, t) \cdot k(x_{i}, x_{i}) }
}
$$
Deci 
$$
\mathbf{\hat{K}}_{t} = \begin{pmatrix}
\frac{-10}{\sqrt{ -3 }}  &  \frac{-2}{\sqrt{ -3 }}  & \frac{-7}{\sqrt{ 6 }}
\end{pmatrix}
$$
c) Determinați eticheta folosind un clasificator SVM dual cu ponderile $\alpha = [1, -2, 3], b = 0.5$ 

Ecuația de predicție este 

$$
\begin{align}
y  & = \mathrm{sgn}(K \alpha^{\top} + b ) =  \\
 & = \mathrm{sgn}  \left( -\frac{10}{\sqrt{ -3 }} + \frac{4}{\sqrt{ -3 }} - \frac{21}{\sqrt{ 6 }} + 0.5 \right) =  \\
 & \mathrm{sgn} \left( -\frac{6}{\sqrt{ -3 }} -\frac{21}{\sqrt{ 6 }} + 0.5 \right)
\end{align}
$$
# Exercițiul 2 

Există o mulțime de minim 3 puncte etichetate în $\mathbb{R}^2$ astfel încât punctele să fie într-o configurație neliniar separabilă în spațiul original și într-o configurație liniar separabilă după aplicarea funcției nucleu $\langle x, z \rangle^2$? .

$$
\begin{align}
k(x, z)  & = \langle x, z \rangle
{ #2}
 =\\
 & = (x_{1}z_{1} + x_{2}z_{1})^2 =  \\
 & = x_{1}^2z_{1}^2 + x_{2}^2z_{2}^2 + 2 x_{1}x_{2}z_{1}z_{2} =  \\
 & =  \langle (x_{1}^2, x_{2}^2, \sqrt{2 }x_{1}x_{2}), (z_{1}^2, z_{2}^2, \sqrt{ 2 }z_{1}z_{2}) \rangle 
\end{align}
$$
Deci avem funcția de scufundare $\phi : \mathbb{R}^2 \to \mathbb{R}^3$
$$
\phi((x_{1}, x_{2})) = (x_{1}^2, x_{2}^2, \sqrt{ 2 }x_{1}x_{2})
$$

Considerăm 3 puncte etichetate care să nu fie separabile liniar. Singura considerație neliniar separabilă este atunci când punctele sunt coliniare și eticheta punctului interior e diferită de eticheta celorlalte puncte. 

Putem considera, deci 

$$
\begin{align}
 & x_{1} = ((-1, 0), -1)  \\ 
 & x_{2} = ((0, 0), 1)  \\
 & x_{3} = ((1, 0), -1)
\end{align}
$$
Aceste puncte sunt coliniare, pe axa $Ox$. 

#### Demonstrăm că nu sunt liniar separabile

Presupunem că există $\mathbf{w} = [w_{1}, w_{2}]$ și $b$ astfel încât:

1) Pentru $x_{1}$ 

$$
\begin{align}
 & w_{1}x_{1} + w_{2}x_{2}+b <0 \iff  \\
\iff  & -w_{1} + 0 + b < 0 \iff b < w_{1}
\end{align}
$$
2) Pentru $x_{2}$ 

$$
0 + 0 + b >0 \iff b > 0
$$

3) Pentru $x_{3}$

$$
w_{1} + b < 0
$$
Deci, adunând 1) cu 3) 

$$
2b <0 \iff b < 0
$$

Contradicție cu 2)!. Deci punctele nu sunt liniar separabile.

#### Scufundare 

În scufundare vom avea 

$$
\begin{align}
 & \phi(x_{1}) = ([1, 0, 0], -1) \\
 & \phi(x_{2}) = ([0, 0, 0], 1)  \\
 & \phi(x_{3}) = ([1, 0, 0], -1)
\end{align}
$$
Punctele asociate lui $x_{1}$ și $x_{3}$ vor coincide, cu aceeași etichetă, și există deci un plan care să poată separa $x_{1}, x_{3}$ de $x_{2}$.

###### Demonstrație 

Fie $\mathbf{w} = [-1, 0, 0], b = 0.5$

Atunci 

$$
y(x_{1}) = \mathrm{sgn}(-1 + 0.5) = \mathrm{sgn}(-0.5) = -1
$$
$$
y(x_{2}) = \mathrm{sgn}(0.5) = 1
$$
$$
y_{3} = \mathrm{sgn}(-1 + 0.5) = -1
$$
Deci punctele sunt liniar separabile

<style> .container {font-family: sans-serif; text-align: center;} .button-wrapper button {z-index: 1;height: 40px; width: 100px; margin: 10px;padding: 5px;} .excalidraw .App-menu_top .buttonList { display: flex;} .excalidraw-wrapper { height: 800px; margin: 50px; position: relative;} :root[dir="ltr"] .excalidraw .layer-ui__wrapper .zen-mode-transition.App-menu_bottom--transition-left {transform: none;} </style><script src="https://cdn.jsdelivr.net/npm/react@17/umd/react.production.min.js"></script><script src="https://cdn.jsdelivr.net/npm/react-dom@17/umd/react-dom.production.min.js"></script><script type="text/javascript" src="https://cdn.jsdelivr.net/npm/@excalidraw/excalidraw@0/dist/excalidraw.production.min.js"></script><div id="Drawing_2026-06-01_1244.40.excalidraw.md1"></div><script>(function(){const InitialData={"type":"excalidraw","version":2,"source":"https://github.com/zsviczian/obsidian-excalidraw-plugin/releases/tag2.23.8","elements":[{"id":"Ujk4mnQkov2-Wi15K428K","type":"line","x":294.20001220703125,"y":166.6875,"width":0,"height":431.1999969482422,"angle":0,"strokeColor":"#1e1e1e","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":2,"strokeStyle":"solid","roughness":1,"opacity":100,"groupIds":[],"frameId":null,"index":"a1","roundness":{"type":2},"seed":885334535,"version":263,"versionNonce":120670569,"isDeleted":false,"boundElements":[],"updated":1780307139050,"link":null,"locked":false,"points":[[0,0],[0,431.1999969482422]],"startBinding":null,"endBinding":null,"startArrowhead":null,"endArrowhead":null,"polygon":false,"hasTextLink":false},{"id":"t2LW5bkL6O3scJqfuxUa3","type":"line","x":92.60000610351562,"y":402.68751525878906,"width":617.6000671386719,"height":2.4000244140625,"angle":0,"strokeColor":"#1e1e1e","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":2,"strokeStyle":"solid","roughness":1,"opacity":100,"groupIds":[],"frameId":null,"index":"a2","roundness":{"type":2},"seed":1036328935,"version":90,"versionNonce":1555473127,"isDeleted":false,"boundElements":[],"updated":1780307145261,"link":null,"locked":false,"points":[[0,0],[617.6000671386719,-2.4000244140625]],"startBinding":null,"endBinding":null,"startArrowhead":null,"endArrowhead":null,"polygon":false,"hasTextLink":false},{"id":"EXuX8q2e51YkFYK7gcS-9","type":"line","x":73.28619631863508,"y":322.10497378804547,"width":602.9858829221171,"height":222.71333307480478,"angle":0,"strokeColor":"#1e1e1e","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":2,"strokeStyle":"solid","roughness":1,"opacity":100,"groupIds":[],"frameId":null,"index":"a3","roundness":{"type":2},"seed":1674554887,"version":401,"versionNonce":1910367280,"isDeleted":false,"boundElements":[],"updated":1780476790751,"link":null,"locked":false,"points":[[0,0],[602.9858829221171,222.71333307480478]],"startBinding":null,"endBinding":null,"startArrowhead":null,"endArrowhead":null,"polygon":false,"hasTextLink":false},{"id":"PbqvPnJb","type":"text","x":305.4000244140625,"y":150.6875,"width":20.3599853515625,"height":25,"angle":0,"strokeColor":"#1e1e1e","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":2,"strokeStyle":"solid","roughness":1,"opacity":100,"groupIds":[],"frameId":null,"index":"a4","roundness":null,"seed":64130983,"version":8,"versionNonce":991614857,"isDeleted":false,"boundElements":[],"updated":1780307160807,"locked":false,"text":"x1","rawText":"x1","fontSize":20,"fontFamily":5,"textAlign":"left","verticalAlign":"top","containerId":null,"originalText":"x1","autoResize":true,"lineHeight":1.25,"hasTextLink":false,"link":null},{"id":"sor1uJgL","type":"text","x":718.2000732421875,"y":386.68751525878906,"width":25.819976806640625,"height":25,"angle":0,"strokeColor":"#1e1e1e","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":2,"strokeStyle":"solid","roughness":1,"opacity":100,"groupIds":[],"frameId":null,"index":"a5","roundness":null,"seed":2031827111,"version":7,"versionNonce":1399344999,"isDeleted":false,"boundElements":[],"updated":1780307164611,"locked":false,"text":"x2","rawText":"x2","fontSize":20,"fontFamily":5,"textAlign":"left","verticalAlign":"top","containerId":null,"originalText":"x2","autoResize":true,"lineHeight":1.25,"hasTextLink":false,"link":null},{"id":"SnZgRoRu","type":"text","x":644.2281905982705,"y":574.8820502878389,"width":23.97998046875,"height":25,"angle":0,"strokeColor":"#1e1e1e","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":2,"strokeStyle":"solid","roughness":1,"opacity":100,"groupIds":[],"frameId":null,"index":"a6","roundness":null,"seed":1620560519,"version":34,"versionNonce":1725101991,"isDeleted":false,"boundElements":[],"updated":1780307919681,"locked":false,"text":"x3","rawText":"x3","fontSize":20,"fontFamily":5,"textAlign":"left","verticalAlign":"top","containerId":null,"originalText":"x3","autoResize":true,"lineHeight":1.25,"hasTextLink":false,"link":null},{"id":"9td8YBm3Bk17kbCpBpMMo","type":"freedraw","x":292.60003662109375,"y":297.8874969482422,"width":0.0001,"height":0.0001,"angle":0,"strokeColor":"#e03131","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":2,"strokeStyle":"solid","roughness":1,"opacity":100,"groupIds":[],"frameId":null,"index":"a8","roundness":null,"seed":1538829001,"version":3,"versionNonce":2024350055,"isDeleted":false,"boundElements":[],"updated":1780307189870,"link":null,"locked":false,"points":[[0,0],[0.0001,0.0001]],"pressures":[],"simulatePressure":true,"hasTextLink":false},{"id":"QSrglq-Yh1PX9TCGposND","type":"freedraw","x":292.60003662109375,"y":297.8874969482422,"width":0.0001,"height":0.0001,"angle":0,"strokeColor":"#e03131","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":2,"strokeStyle":"solid","roughness":1,"opacity":100,"groupIds":[],"frameId":null,"index":"a9","roundness":null,"seed":824128423,"version":3,"versionNonce":816303401,"isDeleted":false,"boundElements":[],"updated":1780307190706,"link":null,"locked":false,"points":[[0,0],[0.0001,0.0001]],"pressures":[],"simulatePressure":true,"hasTextLink":false},{"id":"J6WQ1ar-IOLF8M4ldTaWL","type":"freedraw","x":292.60003662109375,"y":297.8874969482422,"width":0.79998779296875,"height":0.800018310546875,"angle":0,"strokeColor":"#e03131","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":2,"strokeStyle":"solid","roughness":1,"opacity":100,"groupIds":[],"frameId":null,"index":"aA","roundness":null,"seed":1033066217,"version":5,"versionNonce":1693715047,"isDeleted":false,"boundElements":[],"updated":1780307191512,"link":null,"locked":false,"points":[[0,0],[0.79998779296875,0],[0.79998779296875,0.800018310546875],[0.79998779296875,0.800018310546875]],"pressures":[],"simulatePressure":true,"hasTextLink":false},{"id":"dfFDyVoX63R7r0LfIvnZu","type":"freedraw","x":292.60003662109375,"y":296.28749084472656,"width":7.20001220703125,"height":4.800018310546875,"angle":0,"strokeColor":"#e03131","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":2,"strokeStyle":"solid","roughness":1,"opacity":100,"groupIds":[],"frameId":null,"index":"aB","roundness":null,"seed":1417927847,"version":21,"versionNonce":509644297,"isDeleted":false,"boundElements":[],"updated":1780307195914,"link":null,"locked":false,"points":[[0,0],[-0.800048828125,0],[-1.60003662109375,0.800018310546875],[-2.4000244140625,0.800018310546875],[-2.4000244140625,1.600006103515625],[-2.4000244140625,2.4000244140625],[-3.20001220703125,2.4000244140625],[-3.20001220703125,3.20001220703125],[-3.20001220703125,4],[-3.20001220703125,4.800018310546875],[-1.60003662109375,4.800018310546875],[-0.800048828125,4.800018310546875],[0,4.800018310546875],[0.79998779296875,4.800018310546875],[0.79998779296875,3.20001220703125],[2.39996337890625,3.20001220703125],[2.39996337890625,2.4000244140625],[3.199951171875,2.4000244140625],[4,2.4000244140625],[4,2.4000244140625]],"pressures":[],"simulatePressure":true,"hasTextLink":false},{"id":"cJPKXPJWn6xbrvAX-XALF","type":"freedraw","x":293.4000244140625,"y":402.68751525878906,"width":7.20001220703125,"height":6.399993896484375,"angle":0,"strokeColor":"#2f9e44","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":2,"strokeStyle":"solid","roughness":1,"opacity":100,"groupIds":[],"frameId":null,"index":"aC","roundness":null,"seed":1235492167,"version":42,"versionNonce":1752416999,"isDeleted":false,"boundElements":[],"updated":1780307203406,"link":null,"locked":false,"points":[[0,0],[-0.79998779296875,0],[-1.60003662109375,0],[-2.4000244140625,0],[-2.4000244140625,0.79998779296875],[-3.20001220703125,0.79998779296875],[-3.20001220703125,1.5999755859375],[-3.20001220703125,2.399993896484375],[-2.4000244140625,2.399993896484375],[-1.60003662109375,2.399993896484375],[-1.60003662109375,3.199981689453125],[-0.79998779296875,3.199981689453125],[0,4],[0.79998779296875,4],[1.5999755859375,3.199981689453125],[1.5999755859375,2.399993896484375],[1.5999755859375,1.5999755859375],[1.5999755859375,0.79998779296875],[0.79998779296875,0.79998779296875],[0,0],[-1.60003662109375,0],[-4,-0.800018310546875],[-4.79998779296875,-0.800018310546875],[-5.60003662109375,-0.800018310546875],[-5.60003662109375,0],[-4.79998779296875,0],[-4.79998779296875,0.79998779296875],[-4.79998779296875,1.5999755859375],[-4,1.5999755859375],[-3.20001220703125,1.5999755859375],[-3.20001220703125,2.399993896484375],[-2.4000244140625,2.399993896484375],[-2.4000244140625,3.199981689453125],[-2.4000244140625,4],[-1.60003662109375,4.79998779296875],[-1.60003662109375,5.5999755859375],[-0.79998779296875,5.5999755859375],[0,5.5999755859375],[0,4.79998779296875],[0,4],[0,4]],"pressures":[],"simulatePressure":true,"hasTextLink":false},{"id":"Rmo2BjsM","type":"text","x":328.60003662109375,"y":282.18751525878906,"width":57.47996520996094,"height":25,"angle":0,"strokeColor":"#e03131","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":2,"strokeStyle":"solid","roughness":1,"opacity":100,"groupIds":[],"frameId":null,"index":"aD","roundness":null,"seed":214922535,"version":12,"versionNonce":815594983,"isDeleted":false,"boundElements":[],"updated":1780307231360,"locked":false,"text":"x1, x3","rawText":"x1, x3","fontSize":20,"fontFamily":5,"textAlign":"left","verticalAlign":"top","containerId":null,"originalText":"x1, x3","autoResize":true,"lineHeight":1.25,"hasTextLink":false,"link":null},{"id":"44Or4oBP","type":"text","x":334.20001220703125,"y":404.58750915527344,"width":25.819976806640625,"height":25,"angle":0,"strokeColor":"#2f9e44","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":2,"strokeStyle":"solid","roughness":1,"opacity":100,"groupIds":[],"frameId":null,"index":"aE","roundness":null,"seed":1040358057,"version":6,"versionNonce":1261619177,"isDeleted":false,"boundElements":[],"updated":1780307228075,"locked":false,"text":"x2","rawText":"x2","fontSize":20,"fontFamily":5,"textAlign":"left","verticalAlign":"top","containerId":null,"originalText":"x2","autoResize":true,"lineHeight":1.25,"hasTextLink":false,"link":null},{"id":"Ky90uJtj","type":"arrow","x":292.60003662109375,"y":405.8874969482422,"width":0,"height":119.20001220703125,"angle":0,"strokeColor":"#1971c2","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":4,"strokeStyle":"solid","roughness":0,"opacity":100,"groupIds":[],"frameId":null,"index":"aG","roundness":null,"seed":609952807,"version":159,"versionNonce":447582217,"isDeleted":false,"boundElements":[],"updated":1780307314244,"link":null,"locked":false,"points":[[0,0],[0,119.20001220703125]],"startBinding":null,"endBinding":null,"startArrowhead":null,"endArrowhead":"triangle","elbowed":false,"fixedSegments":null,"hasTextLink":false},{"id":"92yBsz9b","type":"text","x":167.800048828125,"y":464.5875701904297,"width":116.91989135742188,"height":25,"angle":0,"strokeColor":"#1971c2","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":4,"strokeStyle":"solid","roughness":0,"opacity":100,"groupIds":[],"frameId":null,"index":"aH","roundness":null,"seed":159406119,"version":111,"versionNonce":882560423,"isDeleted":false,"boundElements":[],"updated":1780307775263,"locked":false,"text":"w= (-1, 0, 0)","rawText":"w= (-1, 0, 0)","fontSize":20,"fontFamily":5,"textAlign":"left","verticalAlign":"top","containerId":null,"originalText":"w= (-1, 0, 0)","autoResize":true,"lineHeight":1.25,"hasTextLink":false,"link":null},{"id":"8wkdeRGL7EgBrEKS7QOM4","type":"freedraw","x":293.4000244140625,"y":350.68751525878906,"width":2.39996337890625,"height":3.20001220703125,"angle":0,"strokeColor":"#1971c2","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":2,"strokeStyle":"solid","roughness":0,"opacity":100,"groupIds":[],"frameId":null,"index":"aL","roundness":null,"seed":676479687,"version":11,"versionNonce":2084870025,"isDeleted":false,"boundElements":[],"updated":1780307377580,"link":null,"locked":false,"points":[[0,0],[0,-0.800018310546875],[0,-2.4000244140625],[0,-3.20001220703125],[0.79998779296875,-3.20001220703125],[1.5999755859375,-1.600006103515625],[1.5999755859375,-0.800018310546875],[2.39996337890625,-0.800018310546875],[2.39996337890625,0],[2.39996337890625,0]],"pressures":[],"simulatePressure":true,"hasTextLink":false},{"id":"PEDqM03ASXo8RkCWVXifW","type":"freedraw","x":295,"y":349.08750915527344,"width":4.79998779296875,"height":3.20001220703125,"angle":0,"strokeColor":"#1971c2","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":2,"strokeStyle":"solid","roughness":0,"opacity":100,"groupIds":[],"frameId":null,"index":"aM","roundness":null,"seed":1971915081,"version":9,"versionNonce":1958883399,"isDeleted":false,"boundElements":[],"updated":1780307378238,"link":null,"locked":false,"points":[[0,0],[-0.79998779296875,0],[-1.5999755859375,-0.800018310546875],[-2.39996337890625,-0.800018310546875],[-3.20001220703125,-1.600006103515625],[-4,-2.399993896484375],[-4.79998779296875,-3.20001220703125],[-4.79998779296875,-3.20001220703125]],"pressures":[],"simulatePressure":true,"hasTextLink":false},{"id":"wMdMtU7vtNvDZObvIVzdf","type":"line","x":55.800048828125,"y":373.8876495361328,"width":315.2000732421875,"height":0.800079345703125,"angle":0,"strokeColor":"#1971c2","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":1,"strokeStyle":"dotted","roughness":0,"opacity":100,"groupIds":[],"frameId":null,"index":"aY","roundness":{"type":2},"seed":1739669705,"version":651,"versionNonce":1919866375,"isDeleted":false,"boundElements":[],"updated":1780307659469,"link":null,"locked":false,"points":[[0,0],[315.2000732421875,-0.800079345703125]],"startBinding":null,"endBinding":null,"startArrowhead":null,"endArrowhead":null,"polygon":false,"hasTextLink":false},{"id":"QZiKnds2PE7ssqNfh4gZ7","type":"line","x":58.20001220703125,"y":370.68751525878906,"width":172.79998779296875,"height":46.400054931640625,"angle":0,"strokeColor":"#1971c2","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":1,"strokeStyle":"dotted","roughness":0,"opacity":100,"groupIds":[],"frameId":null,"index":"aZ","roundness":{"type":2},"seed":1339358311,"version":342,"versionNonce":81247081,"isDeleted":false,"boundElements":[],"updated":1780307669774,"link":null,"locked":false,"points":[[0,0],[172.79998779296875,-46.400054931640625]],"startBinding":null,"endBinding":null,"startArrowhead":null,"endArrowhead":null,"polygon":false,"hasTextLink":false},{"id":"PSQNECoMP3PQimh8h322n","type":"line","x":234.20001220703125,"y":322.68751525878906,"width":318.4000244140625,"height":0.800018310546875,"angle":0,"strokeColor":"#1971c2","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":1,"strokeStyle":"dotted","roughness":0,"opacity":100,"groupIds":[],"frameId":null,"index":"aa","roundness":{"type":2},"seed":1272723753,"version":343,"versionNonce":2093477319,"isDeleted":false,"boundElements":[],"updated":1780307661440,"link":null,"locked":false,"points":[[0,0],[318.4000244140625,-0.800018310546875]],"startBinding":null,"endBinding":null,"startArrowhead":null,"endArrowhead":null,"polygon":false,"hasTextLink":false},{"id":"lKowl6Pypu-aDkY7NK2eI","type":"line","x":548.5999755859375,"y":325.08750915527344,"width":172.79998779296875,"height":45.60003662109375,"angle":0,"strokeColor":"#1971c2","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":1,"strokeStyle":"dotted","roughness":0,"opacity":100,"groupIds":[],"frameId":null,"index":"ab","roundness":{"type":2},"seed":1510716871,"version":355,"versionNonce":1026167527,"isDeleted":false,"boundElements":[],"updated":1780307665770,"link":null,"locked":false,"points":[[0,0],[-172.79998779296875,45.60003662109375]],"startBinding":null,"endBinding":null,"startArrowhead":null,"endArrowhead":null,"polygon":false,"hasTextLink":false},{"id":"91tutRrO3-bwyCq6PrUsl","type":"line","x":233.4000244140625,"y":321.8874969482422,"width":136,"height":53.600006103515625,"angle":0,"strokeColor":"#1971c2","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":1,"strokeStyle":"dotted","roughness":0,"opacity":100,"groupIds":[],"frameId":null,"index":"ac","roundness":{"type":2},"seed":1489810729,"version":142,"versionNonce":1521907177,"isDeleted":false,"boundElements":[],"updated":1780307679562,"link":null,"locked":false,"points":[[0,0],[136,53.600006103515625]],"startBinding":null,"endBinding":null,"startArrowhead":null,"endArrowhead":null,"polygon":false,"hasTextLink":false},{"id":"NRTKC4lyf_wBzzEDlRUOJ","type":"line","x":54.20001220703125,"y":371.4875030517578,"width":496,"height":48.79998779296875,"angle":0,"strokeColor":"#1971c2","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":1,"strokeStyle":"dotted","roughness":0,"opacity":100,"groupIds":[],"frameId":null,"index":"ad","roundness":{"type":2},"seed":1513569351,"version":118,"versionNonce":414834375,"isDeleted":false,"boundElements":[],"updated":1780307685411,"link":null,"locked":false,"points":[[0,0],[496,-48.79998779296875]],"startBinding":null,"endBinding":null,"startArrowhead":null,"endArrowhead":null,"polygon":false,"hasTextLink":false},{"id":"URsxC8dz","type":"text","x":301.40008544921875,"y":351.8874969482422,"width":77.77615356445312,"height":20,"angle":0,"strokeColor":"#1971c2","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":1,"strokeStyle":"dotted","roughness":0,"opacity":100,"groupIds":[],"frameId":null,"index":"ae","roundness":null,"seed":187523111,"version":72,"versionNonce":1031501737,"isDeleted":false,"boundElements":[],"updated":1780307710702,"locked":false,"text":"(0.5, 0, 0)","rawText":"(0.5, 0, 0)","fontSize":16,"fontFamily":5,"textAlign":"left","verticalAlign":"top","containerId":null,"originalText":"(0.5, 0, 0)","autoResize":true,"lineHeight":1.25,"hasTextLink":false,"link":null}],"appState":{"theme":"light","viewBackgroundColor":"#ffffff","currentItemStrokeColor":"#1971c2","currentItemBackgroundColor":"transparent","currentItemFillStyle":"solid","currentItemStrokeWidth":1,"currentItemStrokeStyle":"dotted","currentItemRoughness":0,"currentItemOpacity":100,"currentItemFontFamily":5,"currentItemFontSize":16,"currentItemTextAlign":"left","currentItemStartArrowhead":null,"currentItemEndArrowhead":"triangle","currentItemArrowType":"sharp","currentItemFrameRole":null,"scrollX":-25.927492809295813,"scrollY":-289.39398773283057,"zoom":{"value":1.594394},"currentItemRoundness":"round","gridSize":20,"gridStep":5,"gridModeEnabled":false,"gridColor":{"Bold":"rgba(217, 217, 217, 0.5)","Regular":"rgba(230, 230, 230, 0.5)"},"currentStrokeOptions":null,"frameRendering":{"enabled":true,"clip":true,"name":true,"outline":true,"markerName":true,"markerEnabled":true},"objectsSnapModeEnabled":false,"activeTool":{"type":"selection","customType":null,"locked":false,"fromSelection":false,"lastActiveTool":null},"disableContextMenu":false,"bindingPreference":"enabled","isBindingEnabled":true,"isMidpointSnappingEnabled":true,"boxSelectionMode":"contain"},"files":{}};InitialData.scrollToContent=true;App=()=>{const e=React.useRef(null),t=React.useRef(null),[n,i]=React.useState({width:void 0,height:void 0});return React.useEffect(()=>{i({width:t.current.getBoundingClientRect().width,height:t.current.getBoundingClientRect().height});const e=()=>{i({width:t.current.getBoundingClientRect().width,height:t.current.getBoundingClientRect().height})};return window.addEventListener("resize",e),()=>window.removeEventListener("resize",e)},[t]),React.createElement(React.Fragment,null,React.createElement("div",{className:"excalidraw-wrapper",ref:t},React.createElement(ExcalidrawLib.Excalidraw,{ref:e,width:n.width,height:n.height,initialData:InitialData,viewModeEnabled:!0,zenModeEnabled:!0,gridModeEnabled:!1})))},excalidrawWrapper=document.getElementById("Drawing_2026-06-01_1244.40.excalidraw.md1");ReactDOM.render(React.createElement(App),excalidrawWrapper);})();</script>

| Resursă                                 |      Adresă IP |   Subnet (CIDR) | Tip                         |
| --------------------------------------- | -------------: | --------------: | --------------------------- |
| Pod `flask-deployment-5848f9b768-4rnbd` |  `10.244.0.13` | `10.244.0.0/24` | Pod IP (efemer)             |
| Pod `redis-deployment-6c768c5cfb-92dt6` |   `10.244.0.5` | `10.244.0.0/24` | Pod IP (efemer)             |
| Service `redis-service`                 | `10.96.21.144` |  `10.96.0.0/12` | ClusterIP (virtual, stabil) |
| Service `flask-service`                 | `10.96.82.225` |  `10.96.0.0/12` | ClusterIP (virtual, stabil) |
| Node `k8s-flask-control-plane`          |   `172.21.0.2` | `172.21.0.0/16` | Node IP                     |
|                                         |                |                 |                             |
