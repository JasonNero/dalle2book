# DALL·E-2 Presentation

:::{note}
Author: Jason Schuehlein (js450)  
Email: [js450@hdm-stuttgart.de](mailto:js450@hdm-stuttgart.de)  
:::

## Introduction Text-To-Image

*Image Generation* beschreibt den Task neue Bilder aus einem gelernten Datensatz zu generieren. *Text-To-Image (TTI)* ist ein Subtask und beschreibt *Conditional Image Generation*, also die Generierung von Samples unter der Bedingung eines Labels $p(y|x)$.
*Zero-Shot* TTI geht einen Schritt weiter und ermöglicht auch Generierung von Daten ausserhalb des Trainingsdatensatzes.

Hauefig wird TTI in Verbindung gebracht mit *Generative Adversarial Nets (GAN)*, seit Kurzem jedoch auch verstärkt mit *Denoising Diffusion Models*, worauf u.a. auch DALL·E 2 basiert.

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
        Imagen              :milestone, active, 2022-05-13,
        Parti               :milestone, active, 2022-06-22,
:::

### DALL·E

:::{note}

- [ ] Titelbild des Papers einbinden?
- [ ] Beispielbilder

:::

Der namentliche Vorgaenger zu DALL·E 2 wurde am 5. Januar 2021 in einem [OpenAI Blog Eintrag](https://openai.com/blog/dall-e/) vorgestellt und einige Wochen spaeter, am 24. Februar 2021, wurde das Paper eingereicht mit dem Titel "Zero-Shot Text-to-Image Generation" {cite}`rameshZeroShotTexttoImageGeneration2021`.

DALL·E besteht aus zwei Modulen: dem *Discrete Variational Autoencoder (dVAE)* und einem *Decoder-Only Sparse Transformer*. Letzterer basiert nach eigenen Angaben auf einer Variante von GPT-3.

Lediglich der Code des dVAE Moduls wurde von OpenAI auf GitHub veröffentlicht unter [`openai/DALL-E`](https://github.com/openai/DALL-E). Eine inoffizielle aber komplette Implementierung findet sich hier [`lucidrains/DALLE-pytorch`](https://github.com/lucidrains/DALLE-pytorch).

Öffentlich zugänglich war das Model nie, alternativ kann man aber [CrAIyon](https://www.craiyon.com) bzw. [`borisdayma/dalle-mini`](https://github.com/borisdayma/dalle-mini) verwenden, ein Versuch von {cite:t}`daymaDALLMini2021`, DALL·E zu reproduzieren.

### CLIP

:::{note}

- [ ] Was ist CLIP und wofuer wird es verwendet?
:::

OpenAI's *Contrastive Language Image Pretraining (CLIP)* von wurde am 26. Februar 2021 vorgestellt in "Learning Transferable Visual Models From Natural Language Supervision" von {cite:t}`radfordLearningTransferableVisual2021`.

```{figure} attachments/clip-overview-a.svg
Illustration of the CLIP Training.  
Source: {cite}`radfordLearningTransferableVisual2021`.
```

CLIP besteht aus zwei Encodern, einem der Text in Text Embeddings umwandelt und ein weiterer der Bilder in Bild Embeddings umwandelt.

Fuer einen Batch aus $N$ Image-Text Paaren $(x,y)$ werden alle Text Embeddings allen Image Embeddings gegenübergestellt. CLIP wird nun darauf trainiert die $N$ der $N \times N$ möglichen Image-Text Paare zu identifizieren die korrekt sind. Verwendet wird hierbei die *Cosine Similarity*, diese soll für die korrekten Paare maximiert und die inkorrekten Paare minimiert werden.

### GLIDE

```{figure} attachments/diffusion.gif
---
height: 256px
---

Illustration of the Reverse Diffusion Process.  
Source: [Alex Nichol](https://aqnichol.com/) {cite}`rameshHowDALLWorks`.
```

#### Denoising Diffusion Probabilistic Models (DDPM)

:::{note}

- [ ] Markov Chain mit einbringen
- [ ] ELBO Loss Function einbauen und erklaeren?

:::

Diffusion Modelle sind Generative Modelle, sie generieren Daten ähnlich zu den Trainingsdaten mit denen sie trainiert wurden. Vorgestellt wurden sie von {cite:t}`hoDenoisingDiffusionProbabilistic2020` mit Inspiration aus den "nonequilibrium thermodynamics".

Das Model korrumpiert zuerst die Daten durch Aufaddieren von Gaussian Noise und wird dann darauf trainiert die Daten wiederherzustellen. Gelernt wird also der Reverse Process oder auch Denoising Process, daher auch der Name Denoising Diffusion Probabilistic Models (DDPM). Sobald das Training abgeschlossen ist, kann man neue Daten generieren, indem man einfach Rauschen auf das Model gibt.

Ein grosser Vorteil der Diffusion Modelle gegenüber GANs ist, dass kein Adversarial Training erforderlich ist. Jedoch erfordern sie längere Inferenzzeiten.

## OpenAI DALL·E-2

```{figure} attachments/dalle2explained_polarbear.gif
---
height: 256px
---
DALL·E-2 creating an image of a polarbear playing bass.  
Source: https://openai.com/dall-e-2/.
```

```{figure} attachments/dalle2explained_cat_inpainting.gif
---
height: 256px
---
DALL·E-2 replacing a dog with a cat via Inpainting.  
Source: https://openai.com/dall-e-2/.
```

```{figure} attachments/dalle2explained_mona_inpainting.gif
---
height: 256px
---
DALL·E-2 giving Mona Lisa a mohawk via Inpainting.  
Source: https://openai.com/dall-e-2/.
```

```{figure} attachments/dalle1vs2.png
---
height: 256px
---
"a painting of a fox sitting in a field at sunrise in the style of
Claude Monet”  
Source: https://openai.com/dall-e-2/.
```

### Capabilities and Limitations

:::{note}

- [ ] Examples and abilities going further than just Text-To-Image.
- [ ] nicht gut in variable-binding (spatiale Zuordnungen)
- [ ] Perspektiven
- [ ] Spiegelungen
- [ ] Licht und Schatten
- [ ] Darstellung von Text
- [ ] Geographisches und Zeitliches Wissen
- [ ] Details
:::

### Architecture

#### Prior

#### Decoder

#### Upsampler

### Explorations of the Latent Space

## Further Work

### Google Imagen

:::{note}
Show Benchmarks
:::

### Google Parti

:::{note}
Vergleich herstellen zu GLIDE und DALLE2
:::

## Questions and Discussion

:::{note}
Was wuerdet ihr mit DALLE2 anfangen?
Wo seht ihr potentielle Anwendungen?
:::
