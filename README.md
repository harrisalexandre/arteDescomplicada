# Pintura numerada:
Gere imagens pintadas por número (vetorizadas com SVG) a partir de qualquer imagem de entrada.

## Demonstração:

Experimente [aqui](https://harrisalexandre.github.io/arteDescomplicada/)

### Versão CLI:

A versão CLI é um aplicativo de nó independente que faz a conversão de argumentos, por exemplo:
```
paint-by-numbers-generator-win.exe -i input.png -o output.svg
```
Você pode alterar as configurações em settings.json ou, opcionalmente, especificar um settings.json específico com o argumento `-c path_to_settings.json` .

As configurações contêm basicamente as mesmas configurações da versão web:
 - randomSeed: a semente aleatória para escolher os pontos iniciais do algoritmo de agrupamento k-means. Isso garante que os mesmos resultados sejam gerados todas as vezes.
 - kMeansNrOfClusters: o número de cores para quantizar a imagem.
 - kMeansMinDeltaDifference: a distância delta limite do cluster k-means a ser alcançada antes de parar. Ter um valor maior acelerará o agrupamento, mas poderá produzir clusters abaixo do ideal. Padrão 1.
 - kMeansClusteringColorSpace: o espaço de cores para aplicar clustering.
 - kMeansColorRestrictions: Especifique quais cores devem ser usadas. Uma matriz de valores RGB (como matriz numérica) ou nomes de cores (referência a aliases de cores). Se nenhuma cor for especificada, nenhuma restrição será aplicada. Útil se você tiver apenas algumas cores de tinta em mãos.
 - colorAliases: mapa de chaves/valores onde as chaves são os nomes das cores e os valores são as cores RGB (como array numérico). Você pode usar os nomes das cores nas restrições de cores acima. Os nomes também são mencionados no json de saída que informa quanto% da área é dessa cor específica.

```
	"colorAliases": {
		"A1": [            0,            0,            0        ],
		"A2": [            255,            0,            0        ],
		"A3": [            0,            255,            0        ],
	}
```
 - removeFacetsSmallerThanNrOfPoints: remove quaisquer facetas menores que a quantidade especificada de pixels. Reduzir o valor criará resultados mais detalhados, mas pode ser muito mais difícil de pintar devido ao seu tamanho.
 - removeFacetsFromLargeToSmall (true/false): do maior para o menor evitará que os limites distorçam as formas porque as facetas menores atuam como pontos de ancoragem da borda, mas podem ser consideravelmente mais lentas.
 - MaximumNumberOfFacets: se houver mais facetas do que o número máximo fornecido, continue removendo as menores facetas até que o limite seja atingido.
 
 - nrOfTimesToHalveBorderSegments: reduzir a quantidade de pontos em um segmento de borda (usando a redução de wavelet haar) suavizará mais a curva quadrática, mas com perda de detalhes. Um segmento (borda compartilhada com uma faceta) sempre manterá seu ponto inicial e final.
 
 - strictPixelStripCleanupRuns: a limpeza de pixels estreitos remove faixas de linhas de pixels únicos, o que faria com que algumas facetas tivessem alguns segmentos de bordas que são estreitos demais para serem úteis. A remoção de pequenas facetas pode introduzir novas faixas estreitas de pixels, então isso é repetido em algumas execuções iterativas.
 
 - resizeImageIfTooLarge (true/false): se verdadeiro e a imagem de entrada for maior que as dimensões fornecidas, ela será redimensionada para caber, mas manterá sua proporção.
 - resizeImageWidth: restrição de largura.
 - resizeImageHeight: restrição de altura.

Existem também perfis de saída que você pode definir para gerar o resultado em svg, png, jpg com configurações específicas, por exemplo:
```
  "outputProfiles": [
        {
            "name": "full",
            "svgShowLabels": true,
            "svgFillFacets": true,
            "svgShowBorders": true,
            "svgSizeMultiplier": 3,
            "svgFontSize": 50,
            "svgFontColor": "#333",
            "filetype": "png"
        },
        {
            "name": "bordersLabels",
            "svgShowLabels": true,
            "svgFillFacets": false,
            "svgShowBorders": true,
            "svgSizeMultiplier": 3,
            "svgFontSize": 50,
            "svgFontColor": "#333",
            "filetype": "svg"
        },
        {
            "name": "jpgtest",
            "svgShowLabels": false,
            "svgFillFacets": true,
            "svgShowBorders": false,
            "svgSizeMultiplier": 3,
            "svgFontSize": 50,
            "svgFontColor": "#333",
            "filetype": "jpg",
            "filetypeQuality": 80
        }
    ]
```
Isso define 3 perfis de saída. O perfil "completo" mostra rótulos, preenche as facetas e mostra as bordas com um multiplicador de tamanho 3x, tamanho da fonte 50, cor #333 e saída para uma imagem png. O perfil bordersLabels gera um arquivo svg sem preencher facetas e jpgtest gera um arquivo jpg com configuração de qualidade jpg de 80.

A versão CLI também gera um arquivo json que fornece mais informações sobre a paleta, quais cores são usadas e em que quantidade, por exemplo:
```
  ...
  {
    "areaPercentage": 0.20327615489130435,
    "color": [ 59, 36, 27 ],
    "frequency": 119689,
    "index": 0
  },
   ...
```

A versão CLI é útil se você deseja automatizar o processo em seus próprios scripts.

## Screenshots:

<p>Imagem 1</p>

<div alinhar="center">
<img src="https://github.com/harrisalexandre/pinturapornumeros/assets/50559815/52ebba22-80c2-462f-bb0b-ad5d7ccd57ac" width="500px" />
</div>

<br>

<p>Imagem 2</p>
<div alinhar="center">
<img src="https://github.com/harrisalexandre/pinturapornumeros/assets/50559815/52ebba22-80c2-462f-bb0b-ad5d7ccd57ac" width="500px" />
</div>

## Exemplo de saída:
<p>Imagem 1</p>

<div alinhar="center">
<img src="https://github.com/harrisalexandre/pinturapornumeros/assets/50559815/52ebba22-80c2-462f-bb0b-ad5d7ccd57ac" width="500px" />
</div>

<br>

<p>Imagem 2</p>
<div alinhar="center">
<img src="https://github.com/harrisalexandre/pinturapornumeros/assets/50559815/52ebba22-80c2-462f-bb0b-ad5d7ccd57ac" width="500px" />
</div>

## Executando localmente:

Usei o VSCode, que possui suporte integrado para texto digitado. Para depurar, ele usa um pequeno servidor web para hospedar os arquivos no host local. 

Para executar faça npm installpara restaurar pacotes e então npm startiniciar o servidor web.


## Compilando a versão cli:

Instale o pkg primeiro se ainda não o tiver npm install pkg -g. Em seguida, na pasta raiz, execute pkg .. Isso irá gerar a saída para Linux, Windows e MacOS.
