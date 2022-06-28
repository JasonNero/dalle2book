# DALL·E 2 Presentation

:::{note}
Author: Jason Schuehlein (js450)  
Email: [js450@hdm-stuttgart.de](mailto:js450@hdm-stuttgart.de)  
:::

:::{contents}
:depth: 3
:::

## Introduction Text-To-Image

*Text-To-Image (TTI)* ist ein Subtask der *Bildsynthese* und beschreibt *Conditional Image Generation*, also die Generierung von Bildsamples unter der Bedingung eines Labels $p(x|y)$. *Zero-Shot* TTI geht einen Schritt weiter und ermöglicht auch Generierung von Daten ausserhalb des gelernten Trainingsdatensatzes.

*Generative Adversarial Nets (GAN)* sind häufig Vertreter im Bereich TTI.
Seit einiger Zeit werden aber auch verstärkt *Denoising Diffusion Models* verwendet, dazu später mehr.

Die Qualität von TTI Modellen wird gemessen an:

- **Fidelity**: Übereinstimmung mit den Originalbildern
- **Diversity**: Abdeckung der gesamten Variation der Originalverteilung

Kombiniert kann man das an der sog. *Fréchet inception distance (FID)* ablesen.

:::{admonition} Fréchet inception distance (FID)
:class: dropdown

Die *Fréchet inception distance (FID)* ist eine Metrik zur Beurteilung der Qualität von Bildern generiert durch generative Modelle. Im Vergleich zum *Inception Score (IS)*, wird nicht nur die Verteilung der generierten Bilder betrachtet sondern die Verteilungen der echten sowie generierten Bilder verglichen. FID kombiniert *Fidelity* und *Diversity* in einer Zahl.

Die FID entspricht der quadrierten *Wasserstein Distanz* zwischen zwei multivariaten Gaussverteilungen.
:::

## Recent Work

:::{mermaid}
gantt
    title Recent Publications around Text-To-Image
    dateFormat  YYYY-MM-DD
    axisFormat  %Y-%m
    todaymarker off
    section OpenAI
        DALL·E              :milestone, active, 2021-02-24,
        CLIP                :milestone, active, 2021-02-26,
        Guided Diffusion (Diffusion Models Beat GANs on Image Synthesis) :milestone, 2021-05-11,
        GLIDE               :milestone, active, 2021-12-20,
        DALL·E 2            :milestone, crit, 2022-04-13,
    section Other
        CogView             :milestone, 2021-05-26
        Lafite              :milestone, 2021-11-27,
        Latent Diffusion    :milestone, 2021-12-20,
        Make-A-Scene        :milestone, 2022-03-24,
        VQGAN-CLIP          :milestone, 2022-04-18,
        CogView 2           :milestone, 2022-04-28,
    section Google
        Imagen              :milestone, 2022-05-13,
        Parti               :milestone, 2022-06-22,
:::

### DALL·E

Der namentliche Vorgänger zu DALL·E 2 wurde am 5. Januar 2021 in einem [OpenAI Blog Eintrag](https://openai.com/blog/dall-e/) vorgestellt und mehrere Wochen später, am 24. Februar 2021, wurde das Paper eingereicht mit dem Titel ["Zero-Shot Text-to-Image Generation"](http://arxiv.org/abs/2102.12092) {cite}`rameshZeroShotTexttoImageGeneration2021`.

DALL·E besteht aus zwei Modulen: dem *Discrete Variational Autoencoder (dVAE)* und einem *Decoder-Only Sparse Transformer*. Letzterer basiert nach eigenen Angaben auf einer Variante von GPT-3.

Lediglich der Code des dVAE Moduls wurde von OpenAI auf GitHub veröffentlicht unter [`openai/DALL-E`](https://github.com/openai/DALL-E). Eine inoffizielle aber komplette Implementierung findet sich hier [`lucidrains/DALLE-pytorch`](https://github.com/lucidrains/DALLE-pytorch).

Öffentlich zugänglich war das Model nie, alternativ kann man aber [CrAIyon](https://www.craiyon.com) bzw. [`borisdayma/dalle-mini`](https://github.com/borisdayma/dalle-mini) verwenden, ein Versuch von {cite:t}`daymaDALLMini2021`, DALL·E zu reproduzieren.

Folgende Eigenschaften von DALL·E sind herauszustellen:

- **Variable Binding**: Korrektes Zuweisen von Attributen zu Objekten ({numref}`dalle1-var-fig`)
- Mehrere Objekte in einem Bild ({numref}`dalle1-var-fig`)
- Beachtung von Perspektive ({numref}`dalle1-persp-fig`)
- Interne und Externe Objektstrukturen ({numref}`dalle1-struct-fig`)
- Zeitliches und geographisches Wissen

```{figure} attachments/dalle1_variable_binding.png
:name: dalle1-var-fig
DALL·E - Multiple Objects and Variable Binding Examples.  
https://openai.com/blog/dall-e/.
```

:::::{grid}
:gutter: 2

::::{grid-item}
:::{figure} attachments/dalle1_perspective.png
:name: dalle1-persp-fig

DALL·E - Perspective Examples.  
https://openai.com/blog/dall-e/.
:::
::::

::::{grid-item}
:::{figure} attachments/dalle1_structures.png
:name: dalle1-struct-fig

DALL·E - Perspective Examples.  
https://openai.com/blog/dall-e/.
:::
::::
:::::

(CLIP)=
### CLIP

OpenAI's *Contrastive Language Image Pretraining (CLIP)* von wurde am 26. Februar 2021 vorgestellt in ["Learning Transferable Visual Models From Natural Language Supervision"](http://arxiv.org/abs/2103.00020) von {cite:t}`radfordLearningTransferableVisual2021`.

```{figure} attachments/clip-overview-a.svg
Illustration of the CLIP Training.  
{cite}`radfordLearningTransferableVisual2021`.
```

CLIP besteht aus zwei Encodern, einem der Text in Text Embeddings umwandelt und ein weiterer der Bilder in Bild Embeddings umwandelt.

Für einen Batch aus $N$ Image-Text Paaren $(x,y)$ werden alle Text Embeddings allen Image Embeddings gegenübergestellt. CLIP wird nun darauf trainiert die $N$ der $N \times N$ möglichen Image-Text Paare zu identifizieren die korrekt sind. Verwendet wird hierbei die *Cosine Similarity*, diese soll für die korrekten Paare maximiert und die inkorrekten Paare minimiert werden.

Effektiv lernt CLIP ein *"joint representation space"* für Texte und Bilder.

````{admonition} Refresher: Cosine Similarity
:class: dropdown
Das Skalarprodukt

```{math}
\mathbf{a} \cdot \mathbf{b} = \| \mathbf{a} \|_2 \| \mathbf{b} \|_2 \cos{\theta}
```

wird umgeformt um die Cosine Similarity $S_C$ zu erhalten

```{math}
S_C(\mathbf{a}, \mathbf{b}) := \cos{\theta} = \frac{\mathbf{a} \cdot \mathbf{b}}{\| \mathbf{a} \|_2 \| \mathbf{b} \|_2}
```
````

(GLIDE)=
### GLIDE

```{figure} attachments/glide_header.png

Cherrypicked generations of GLIDE {cite}`nicholGLIDEPhotorealisticImage2022`.
```

GLIDE ist ein weiteres TTI Model von OpenAI und wurde am 20. Dezember 2021 in dem gleichnamigen Paper ["GLIDE: Towards Photorealistic Image Generation and Editing with Text-Guided Diffusion Models"](http://arxiv.org/abs/2112.10741) von {cite:t}`nicholGLIDEPhotorealisticImage2022` vorgestellt.

Zwar bekam GLIDE keinen eigenen Blog Eintrag auf der OpenAI Website, dafür wurde aber der Code offiziell auf GitHub veröffentlicht unter [`openai/glide-text2im`](https://github.com/openai/glide-text2im). Neben Checkpoints für eine kleinere Version, genannt *"GLIDE (filtered)"*, gibt es dort auch Notebooks zur Inferenz von Text-To-Image und Inpainting.

Trainiert wurde diese kleine Version mit aggressiv gefilterten Trainingsdaten, um Gewalt und Hass Symbole zu entfernen. Sogar Humanoiden wurden komplett entfernt. Durch die relativ kleine Grösse scheitert GLIDE (filtered) jedoch häufig an Variable Binding.

#### Basics of Denoising Diffusion Probabilistic Models (DDPM)

Diffusion Modelle sind, wie auch GANs, Generative Modelle, sie generieren Daten ähnlich zu den Trainingsdaten mit denen sie trainiert wurden. Beschrieben wurden sie u.a. von {cite:t}`hoDenoisingDiffusionProbabilistic2020` mit Inspiration aus den "nonequilibrium thermodynamics".

```{figure} attachments/diffusion_markov.png
:name: markov-fig

Forward $q(x_t|x_{t-1})$ and Backwards $p_{\theta}(x_{t-1}|x_t)$ Diffusion Process as a Markov Chain. {cite}`hoDenoisingDiffusionProbabilistic2020`
```

Ein Diffusion Model ist prinzipiell eine Markov Chain. In jedem Zeitschritt werden die Daten durch iteratives Aufaddieren von Gaussian Noise korrumpiert, bis nur noch Rauschen übrig ist. Das Netz wird dann darauf trainiert die Daten wiederherzustellen. Gelernt wird also der Reverse Diffusion Process oder auch Denoising, daher auch der Name Denoising Diffusion Probabilistic Models (DDPM). Sobald das Training abgeschlossen ist, kann man neue Daten generieren, indem man Rauschen auf das trainierte Model gibt und den Reverse Process ablaufen lässt.

Ein grosser Vorteil der Diffusion Modelle gegenüber GANs ist, dass kein Adversarial Training erforderlich ist. Ausserdem ist die Implementierung seht flexibel, denn die einzige Anforderung an die darunterliegende Architektur ist, dass Input und Output gleich gross sind.

Nachteil der Diffusion Modelle gegenüber GANs sind jedoch die längeren Inferenzzeiten.

:::{figure} attachments/denoising_different_steps.png

DDPM applied on CelebA-HQ, showing the Reverse Process starting at different timesteps {cite}`hoDenoisingDiffusionProbabilistic2020`
:::

## DALL·E 2

```{figure} attachments/dalle2_header.png

Cherrypicked generations of DALL·E 2 {cite}`rameshHierarchicalTextConditionalImage2022`.
```

```{figure} attachments/dalle2explained_polarbear.gif
:height: 256px

DALL·E 2 creating an image of a polarbear playing bass.  
https://openai.com/dall-e-2/.
```

**DALL·E 2** wurde von OpenAI entwickelt und mit dem [Blog Post](https://openai.com/dall-e-2/) des Projekts am *06. April 2022* veroeffentlicht. Das dazugehörige Paper *[Hierarchical Text-Conditional Image Generation with CLIP Latents](https://arxiv.org/abs/2204.06125)* von {cite:t}`rameshHierarchicalTextConditionalImage2022` wurde eine Woche später am *13. April 2022* vorgelegt und beschreibt **unCLIP**, die grundlegende Architektur hinter DALL·E 2. Das Deployment dessen, die sog. **DALL·E 2 Preview** ist eine modifizierte *"production version"* {cite}`rameshHierarchicalTextConditionalImage2022`.

DALL·E 2 war bis zur Veröffentlichung von Googles Imagen State of the Art im Bereich Text-To-Image.

Im Vergleich zu seinem namentlichen Vorgänger hat DALL·E 2 eine höhere Auflösung und ein höheres Level an Photorealismus (siehe {numref}`dalle1-fox-fig` vs. {numref}`dalle2-fox-fig`).

:::::{grid}
:gutter: 2

::::{grid-item}
:::{figure} attachments/dalle1_fox.jpg
:name: dalle1-fox-fig

DALL·E 1: "a painting of a fox sitting in a field at sunrise in the style of Claude Monet”
https://openai.com/dall-e-2/.
:::
::::

::::{grid-item}
:::{figure} attachments/glide_fox.png
:name: glide-fox-fig

GLIDE: "a painting of a fox in the style of starry night” {cite}`nicholGLIDEPhotorealisticImage2022`
:::
::::
::::{grid-item}
:::{figure} attachments/dalle2_fox.jpg
:name: dalle2-fox-fig

DALL·E 2: "a painting of a fox sitting in a field at sunrise in the style of Claude Monet”
https://openai.com/dall-e-2/.
:::
::::
:::::

Neben *Text-to-Image* besitzt DALL·E 2 aber auch noch weitere Fähigkeiten:

- Editieren von Bildern durch **Inpainting** (siehe {numref}`inpainting1-fig` und {numref}`inpainting2-fig`),
- **Varianten** eines Input Bildes erzeugen (siehe {numref}`variations-fig`) und
- **Text Diffs** - Durch Text gesteuerte Manipulation im Latent Space (siehe {numref}`textdiff-fig`).

:::::{grid}
:gutter: 2

::::{grid-item}
:columns: 6

:::{figure} attachments/dalle2explained_cat_inpainting.gif
:name: inpainting1-fig

DALL·E 2 replacing a dog with a cat via Inpainting.  
https://openai.com/dall-e-2/.
:::
::::
::::{grid-item}
:columns: 6

:::{figure} attachments/dalle2explained_mona_inpainting.gif
:name: inpainting2-fig

DALL·E 2 giving Mona Lisa a mohawk via Inpainting.  
https://openai.com/dall-e-2/.
:::
::::
:::::

:::{figure} attachments/dalle2_fox_variants.png
:name: variations-fig

DALL·E 2 Variations on "a painting of a fox sitting in a field at sunrise in the style of Claude Monet". Created by myself with DALL·E 2: https://labs.openai.com/s/fb5LwnfumVDJ5NbNFiql16XJ
:::

:::{figure} attachments/textdiff_house.gif
:name: textdiff-fig
:height: 256px

DALL·E 2 Textdiff {cite}`rameshHowDALLWorks`: $(\text{image of victorian house}) + \text{"a modern house"} − \text{"a victorian house"}$
:::

Initial bekamen 400 ausgewählte Personen Zugriff auf eine API über die Inferenzen durchgeführt werden können und über eine Waitlist werden weitere Nutzer zugelassen. Stand 18. Mai 2022 wurden bereits 3 Millionen Bilder generiert und es sollen $\approx 1000$ neue Nutzer pro Woche freigeschaltet werden {cite}`DALLResearchPreview2022`.

Grund für den beschränkten Zugang sind Sicherheitsbedenken seitens OpenAI {cite}`mishkinDALLPreviewRisks2022`. Vermutlich aber auch Interesse an kommerzieller Verwertung wie bereits auch GPT3.

```{figure} attachments/dalle2_figure2.png
---
name: dalle2-architecture-fig
---

Overview of DALL·E 2 {cite}`rameshHierarchicalTextConditionalImage2022`.
```

In {numref}`dalle2-architecture-fig` ist die grundlegende Architektur dargestellt, sie besteht aus 3 Elementen:

- **CLIP Model** um Text Embeddings zu generieren
- **Prior** um Text Embeddings in Image Embeddings umzuwandeln
- **Decoder** um aus Image Embeddings ein Bild zu generieren

Tatsächlich ist DALL·E 2 eher eine Weiterentwicklung von GLIDE ({numref}`glide-fox-fig`). Die eigentliche Neuerung besteht darin, dass ein Diffusion Model statt auf Text Encodings, nun auf CLIP Image Embeddings trainiert wird. Aus diesem "Umweg" resultieren vorteilhafte Eigenschaften, so kann man beispielsweise den CLIP Latent Space erkunden und visualisieren, daher auch der Name *"unCLIP"*.

Ähnlich wie bei DALL·E, wurde kein Code des Models veröffentlicht, deshalb wurde von der Open-Source Community ein Versuch gestartet, DALL·E 2 (bzw. das im Paper spezifizierte unCLIP) zu reproduzieren, es gibt also eine inoffizielle Implementierung in dem GitHub Repository [`lucidrains/DALLE2-pytorch`](https://github.com/lucidrains/DALLE2-pytorch).
Diese ist grösstenteils abgeschlossen, und Stand 25. Juni 2022 trainiert die Community des AI Vereins [LAION](https://laion.ai/#top) einen ersten [Prior](https://huggingface.co/zenglishuci/conditioned-prior) und auch erste [Decoder](https://huggingface.co/Veldrovive/DA-VINC-E) sind in der Community zu finden.

Ein erstes Inferenz Notebook der Community welches findet sich unter [`LAION-AI/dalle2-laion`](https://github.com/LAION-AI/dalle2-laion).

```{figure} attachments/laion_dalle2_fox.png

Inference on the same fox prompt as above, using [`LAION-AI/dalle2-laion`](https://github.com/LAION-AI/dalle2-laion). Inference Time: ~10min (Tesla T4).
```

[ThisImageDoesNotExist](https://thisimagedoesnotexist.com) ist eine kleine Demo bei der man raten muss welche Bilder von einem Menschen sind und welche von DALL·E 2 generiert wurden. Der Durchschnitt liegt bei $18/30$ korrekt zugeordneten Bildern.

### Access

Unter den 400 Auserwählten waren zu Beginn nur 200 OpenAI Mitarbeiter, 10 Künstler, "ein paar Dutzend" anerkannte Wissenschaftler und 165 "company friends". Über eine Waitlist wurden über Zeit auch weiteren Personen eingeladen, bis zu 1.000 Personen pro Woche {cite}`DALLResearchPreview2022`.

Die Verwendung der API ist nur für persönliche, nicht-kommerzielle oder wissenschaftliche Zwecke zulässig. User müssen beim Posten von generierten Bildern eindeutig kennzeichnen ob und welcher Teil des Bildes von DALL·E 2 generiert wurde. Ausserdem enthält jedes generierte Bild ein kleines Wasserzeichen in der unteren rechten Bildecke (siehe {numref}`signature-fig`).

```{figure} attachments/signature_closeup.png
---
name: signature-fig
---
The DALL·E 2 signature present in every image it creates via the API {cite}`mishkinDALLPreviewRisks2022`.
```

Ausserdem filtert die API Input nach Kriterien wie z.B. Sicherheitsbedenken (sexualisierte oder suggestive Bilder von Kindern, Gewalt, politischer Content und "toxischer Content").

Allerdings geschieht diese Filtering von Input Text und Input Bild unabhängig voneinander. Demnach könnte man das Model anweisen Inpainting für ein Bild einer Dusche mit dem Text "a woman" zu machen, und dabei potenziell ein Bild einer nackten Frau generieren.

### Limitations

Trotz des grossen Fortschritts in Fidelity und Diversity der Samples die DALL·E 2 generieren kann, hat es auch einige Limitationen. Ganz besonders ist es schlechter als GLIDE im Bereich Variable Binding (siehe {numref}`dalle2-variable-fig`). Die Hypothese der Autoren ist dass CLIP selbst keine expliziten Attributs Zuweisung modellieren kann.
Aber auch detailreichere Szenen oder zusammenängenden Text (siehe {numref}`dalle2-text-fig`) schafft unCLIP nicht korrekt darzustellen.

```{figure} attachments/dalle2_figure15.png
:name: dalle2-variable-fig

unCLIP confusing variable assignments {cite}`rameshHierarchicalTextConditionalImage2022`.
```

```{figure} attachments/dalle2_figure16.png
:name: dalle2-text-fig

unCLIP prompted with: "A sign that says deep learning." {cite}`rameshHierarchicalTextConditionalImage2022`.
```

In {numref}`dalle2-compared-fig` sieht man eine groessere Gegenueberstellung der bisherigen Text To Image Modelle.

```{figure} attachments/dalle2_figure12.png
:name: dalle2-compared-fig

DALL·E vs GLIDE vs unCLIP on COCO {cite}`rameshHierarchicalTextConditionalImage2022`
```

### Architecture

Wie bereits beschrieben besteht DALL·E 2 aus 2 Komponenten:

- Der *Prior* $P(z_i|y)$ erzeugt CLIP Image Embeddings $z_i$ unter gegebener Caption $y$.
- Der *Decoder* $P(x|z_i,y)$ erzeugt Bilder $x$ unter gegebenen Image Embeddings $z_i$ (und optional auch der Caption $y$, bleibt aber hier ungenutzt).

Beide zusammen ergeben ein Generative Model $P(x|y)$ für die Bilder $x$ mit gegebenen Captions $y$. $P(x|y)$ kann geschrieben werden als

```{math}
P( x | y ) = P(x, z_i | y),
```

weil $z_i$ eine deterministische Funktion von $x$ ist. Weiter kann man mit der Kettenregel umformen und erhält

```{math}
P( x | y ) =  P(x, z_i | y) = P(x | z_i, y)P(z_i|y)

```

Man kann also aus der echten Verteilung $P(x|y)$ samplen, indem man erst $z_i$ mit dem Prior sampled und dann $x$ mit dem Decoder.

Was bisher nicht erwähnt wurde sind die 2 Upsampling Diffusion Models:

- $64 \times 64 \rightarrow 256 \times 256$ und
- $256 \times 256 \rightarrow 1024 \times 1024$

```{figure} attachments/hyperparameters.png
:name: hyperparams-fig

Hyperparameters {cite}`rameshHierarchicalTextConditionalImage2022`
```

#### Prior

Der Prior generiert aus dem Labeln $y$ ein CLIP Image Embedding $z_i$.
Hierfür haben die Autoren zwei verschiedene Model Klassen getestet, einen *Autoregressive (AR)* Prior und einen *Diffusion* Prior. Letzterer wurde als effizienter und qualitativ hochwertiger befunden und im Folgenden genauer betrachtet.

##### Diffusion Prior

Wie bereits im {ref}`GLIDE` Kapitel erwähnt ist eine Anforderung an Diffusion Modellen, dass Input und Output die gleiche Grösse haben, bei DALL·E 2 wird hier ein *Decoder-Only Transformer* mit *Casual Attention Mask* verwendet.

Die Sequenz auf welcher der Transformer agiert besteht aus:

- Tokenization des Text
- CLIP Text Embedding der Tokens
- Embedding des Diffusion Timesteps
- Noised CLIP Image Embedding
- "Final Embedding" welches verwendet wird um das Denoised CLIP Image Embedding vorherzusagen

Um die Qualität zu verbessern, werden bei jedem Sampling je 2 $z_i$ Samples generiert und das Sample ausgewählt das ein höheres $z_i \cdot z_t$ aufweist. Ein höheres Skalarprodukt der beiden Embeddings bedeutet dass die Caption das Bild besser beschreibt (vgl. Training von {ref}`CLIP`).

Der verwendete Loss ist eine Vereinfachung von der von {cite:t}`hoDenoisingDiffusionProbabilistic2020` verwendeten Loss Function bei DDPM, es wird lediglich der Mean Squared Error zwischen echtem $z_i$ und der Vorhersage berechnet:

```{math}
L_{prior} = \mathbb{E}_{t \sim [1,T], z_i^{(t)} \sim q_t} [\|  f_{\theta}(z_i^{(t)}, t, y) - z_i \|^2]
```

#### Decoder

```{note}

- [ ] UNet kurz erwaehnen, schliesslich heisst das Kapitel Architektur
```

Der Decoder der in DALL·E 2 Verwendung findet ist eine leichte Modifikation des Diffusions Models {ref}`GLIDE` von {cite:t}`nicholGLIDEPhotorealisticImage2022`.
Zur Erinnerung: GLIDE generiert Bilder direkt aus Text Embeddings, ohne Umweg über CLIP Embeddings.
Um diese Embeddings nun bei gleichbleibender Architektur in den Decoder einzubringen, werden die sie in das existierende Timestep Embedding projiziert. Ausserdem werden sie auch noch in 4 extra Token projiziert die an die Text Tokens des GLIDE Text Encoders angehängt werden.

Die Autoren entschieden sich für das Beibehalten des "*text-conditioning pathway*" des GLIDE Decoders unter der Annahme, dass das Diffusion Model dadurch Aspekte von Natural Language lernen könnte, die CLIP nicht erfasst, wie beispielsweise Variable Binding. Letztendlich wird er in DALL·E 2 aber nicht verwendet, weil er zu keinen merklichen Verbesserungen führte.

Durch sog. *Classifier-free Guidance* wurde die Sampling Qualität weiter verbessert, vorgestellt wurde diese Technik von {cite:t}`hoClassifierFreeDiffusionGuidance2021`.

#### Upsampler

Die Diffusion Upsampler ($64^2 \rightarrow 256^2$ und $256^2 \rightarrow 1024^2$) folgen der *Unconditional ADMNet* Architektur aus ["Diffusion Models Beat GANs on Image Synthesis"](http://arxiv.org/abs/2105.05233) von {cite:t}`dhariwalDiffusionModelsBeat2021`.

Trainiert wurden die ADMNets auf leicht korrumpierten Bildern um die Qualität des Upsamplings zu verbessern. In Experimenten hat sich ausserdem gezeigt, dass eine Konditionierung auf die Bild Captions keinen sichtbaren Verbesserungen mit sich bringt.

### Training Dataset

Ähnlich wie schon bei GLIDE wurden die Trainingsdaten auf denen das Model trainiert wurde, gefiltert, um "explicit content" gering zu halten. Diesmal jedoch weniger aggressiv: Bilder und Captions die "grafische sexuelle sowie gewalttätige Inhalte" haben wurden entfernt, ebenso wie "Hate Symbols".

Der Trainingsdatensatz besteht aus 2 Teilen:

- dem CLIP Datensatz $\approx 400M$ {cite}`radfordLearningTransferableVisual2021`
- und dem DALL·E Datensatz $\approx 250M$ {cite}`rameshZeroShotTexttoImageGeneration2021`

Die CLIP Encoder wurden auf beiden trainiert und dann eingefroren. Prior, Decoder und Upsampler hingegen nur auf letzterem.

## Further Work

### Google Imagen

:::{figure} attachments/imagen_header.png

A cherrypicked collection of Imagen generations {cite}`sahariaPhotorealisticTexttoImageDiffusion2022`.
:::

Googles Imagen wurde am 13. Mai 2022 in dem Paper [Photorealistic Text-to-Image Diffusion Models with Deep Language Understanding](http://arxiv.org/abs/2205.11487) {cite}`sahariaPhotorealisticTexttoImageDiffusion2022` vorgestellt und hat DALL·E 2 als State of The Art Model abgeloest.

:::{figure} attachments/imagen_overview.png
:height: 512px

Overview of the Imagen architecture {cite}`sahariaPhotorealisticTexttoImageDiffusion2022`.
:::

Imagen verzichtet hier also auf das "Layer of Indirection" über die CLIP Image Embeddings und trainiert auf einem mächtigen Text Encoder. Ausserdem sind hier die Super-Resolution Modelle tatsächlich Text-Conditional.

:::{figure} attachments/imagen_dalle2_glide_backpack.png

Imagen vs DALL·E 2 vs GLIDE: "A black apple and a green backpack.".  
Adapted from {cite}`sahariaPhotorealisticTexttoImageDiffusion2022`.
:::

:::{figure} attachments/imagen_dalle2_glide_text.png

Imagen vs DALL·E 2 vs GLIDE: "A storefront with Text to Image written on it.".  
Adapted from {cite}`sahariaPhotorealisticTexttoImageDiffusion2022`.
:::

## Questions and Discussion

:::{note}

Was wuerdet ihr mit DALL·E 2 anfangen?
:::
