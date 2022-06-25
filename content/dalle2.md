# DALL·E 2 Presentation

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

OpenAI's *Contrastive Language Image Pretraining (CLIP)* von wurde am 26. Februar 2021 vorgestellt in "Learning Transferable Visual Models From Natural Language Supervision" von {cite:t}`radfordLearningTransferableVisual2021`.

```{figure} attachments/clip-overview-a.svg
Illustration of the CLIP Training.  
{cite}`radfordLearningTransferableVisual2021`.
```

CLIP besteht aus zwei Encodern, einem der Text in Text Embeddings umwandelt und ein weiterer der Bilder in Bild Embeddings umwandelt.

Fuer einen Batch aus $N$ Image-Text Paaren $(x,y)$ werden alle Text Embeddings allen Image Embeddings gegenübergestellt. CLIP wird nun darauf trainiert die $N$ der $N \times N$ möglichen Image-Text Paare zu identifizieren die korrekt sind. Verwendet wird hierbei die *Cosine Similarity*, diese soll für die korrekten Paare maximiert und die inkorrekten Paare minimiert werden.

### GLIDE

```{figure} attachments/diffusion.gif
---
height: 256px
---

Illustration of the Reverse Diffusion Process.  
[Alex Nichol](https://aqnichol.com/) {cite}`rameshHowDALLWorks`.
```

#### Denoising Diffusion Probabilistic Models (DDPM)

:::{note}

- [ ] Markov Chain mit einbringen
- [ ] ELBO Loss Function einbauen und erklaeren?

:::

Diffusion Modelle sind Generative Modelle, sie generieren Daten ähnlich zu den Trainingsdaten mit denen sie trainiert wurden. Vorgestellt wurden sie von {cite:t}`hoDenoisingDiffusionProbabilistic2020` mit Inspiration aus den "nonequilibrium thermodynamics".

Das Model korrumpiert zuerst die Daten durch Aufaddieren von Gaussian Noise und wird dann darauf trainiert die Daten wiederherzustellen. Gelernt wird also der Reverse Process oder auch Denoising Process, daher auch der Name Denoising Diffusion Probabilistic Models (DDPM). Sobald das Training abgeschlossen ist, kann man neue Daten generieren, indem man einfach Rauschen auf das Model gibt.

Ein grosser Vorteil der Diffusion Modelle gegenüber GANs ist, dass kein Adversarial Training erforderlich ist. Jedoch erfordern sie längere Inferenzzeiten.

## OpenAI DALL·E 2

```{figure} attachments/dalle2explained_polarbear.gif
---
height: 256px
---

DALL·E 2 creating an image of a polarbear playing bass.  
https://openai.com/dall-e-2/.
```

**DALL·E 2** wurde von OpenAI entwickelt und mit dem [Blog Post](https://openai.com/dall-e-2/) des Projekts am *06. April 2022* veroeffentlicht. Das dazugehörige Paper *[Hierarchical Text-Conditional Image Generation with CLIP Latents](https://arxiv.org/abs/2204.06125)* von {cite:t}`rameshHierarchicalTextConditionalImage2022` wurde eine Woche später am *13. April 2022* vorgelegt und beschreibt **unCLIP**, die grundlegende Architektur hinter DALL·E 2. Das Deployment dessen, die sog. **DALL·E 2 Preview** ist eine modifizierte *"production version"* {cite}`rameshHierarchicalTextConditionalImage2022`.

DALL·E 2 war bis zur Veröffentlichung von Googles Imagen State of the Art im Bereich Text-To-Image.

```{figure} attachments/dalle1vs2.png
---
name: dalle1vs2-fig
height: 256px
---

DALL·E-1 vs. DALL·E 2: "a painting of a fox sitting in a field at sunrise in the style of Claude Monet”  
https://openai.com/dall-e-2/.
```

Im Vergleich zu seinem namentlichen Vorgänger hat DALL·E 2 eine höhere Auflösung und ein höheres Level an Photorealismus (siehe {numref}`dalle1vs2-fig`).
Es besitzt aber neben *Text-to-Image* auch noch weitere Fähigkeiten:

- Editieren von Bildern durch **Inpainting** (siehe {numref}`inpainting1-fig` und {numref}`inpainting2-fig`),
- **Varianten** eines Input Bildes erzeugen und
- **Text Diffs** anwenden um ein Bild zu manipulieren.

```{figure} attachments/dalle2explained_cat_inpainting.gif
---
height: 256px
name: inpainting1-fig
---

DALL·E 2 replacing a dog with a cat via Inpainting.  
https://openai.com/dall-e-2/.
```

```{figure} attachments/dalle2explained_mona_inpainting.gif
---
height: 256px
name: inpainting2-fig
---

DALL·E 2 giving Mona Lisa a mohawk via Inpainting.  
https://openai.com/dall-e-2/.
```

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

Ähnlich wie bei DALL·E, wurde kein Code des Models veröffentlicht, deshalb wurde von der Open-Source Community ein Versuch gestartet, DALL·E 2 (bzw. das im Paper spezifizierte unCLIP) zu reproduzieren, es gibt also eine inoffizielle Implementierung in dem GitHub Repository [`lucidrains/DALLE2-pytorch`](https://github.com/lucidrains/DALLE2-pytorch).
Diese ist grösstenteils abgeschlossen, und Stand 25. Juni 2022 trainiert die Community des AI Vereins [LAION](https://laion.ai/#top) einen ersten [Prior](https://huggingface.co/zenglishuci/conditioned-prior) und auch erste Testruns des [Decoders](https://wandb.ai/veldrovive/dalle2_train_decoder) sind in der Community zu finden.

### API Access

Unter den Auserwählten waren zu Beginn nur 200 OpenAI Mitarbeiter, 10 Künstler, "ein paar Dutzend" anerkannte Wissenschaftler und 165 "company friends". Über eine Waitlist wurden über Zeit auch weiteren Personen eingeladen, bis zu 1.000 Personen pro Woche {cite}`DALLResearchPreview2022`.

Die Verwendung der API ist nur für persönliche, nicht-kommerzielle oder wissenschaftliche Zwecke zulässig. User müssen beim Posten von generierten Bildern eindeutig kennzeichnen ob und welcher Teil des Bildes von DALL·E 2 generiert wurde. Ausserdem enthält jedes generierte Bild ein kleines Wasserzeichen in der unteren rechten Bildecke (siehe {numref}`signature-fig`)

```{figure} attachments/signature_closeup.png
---
name: signature-fig
---
The DALL·E 2 signature present in every image it creates.
```

Neben dem oben genannten Filtering auf den Trainingsdaten wird bei der API auch der Input gefiltert nach Kriterien wie "Sicherheitsbedenken" (sexualisierte oder suggestive Bilder von Kindern, Gewalt, politischer Content und "toxischer Content").

Allerdings geschieht das Filtering von Input Text und Bild unabhängig voneinander. Demnach könnte man das Model anweisen Inpainting für ein Bild einer Dusche mit dem Text "a woman" zu machen, und dabei potenziell ein Bild einer nackten Frau generieren.

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

CLIP (ViT-H/16)

#### Prior

#### Decoder and Upsampler

Der Decoder der in DALL·E 2 Verwendung findet ist eine leichte Modifikation des Diffusions Models GLIDE von {cite:t}`nicholGLIDEPhotorealisticImage2022`.

### Training

Die Trainingsdaten auf denen OpenAI das Model trainiert hat, wurden gefiltert um "explicit content" gering zu halten. Bilder und Captions die "grafische sexuelle sowie gewalttätige Inhalte" haben wurden entfernt, ebenso wie "Hate Symbols". Bei OpenAIs GLIDE wurde ein ähnliches, aber aggressiveres Filtering durchgeführt bei dem auch Menschen im Allgemeinen entfernt wurden.
Der Trainingsdatensatz besteht aus 2 Teilen:

- dem CLIP Datensatz $\approx 400M$ {cite}`radfordLearningTransferableVisual2021`
- und dem DALL·E Datensatz $\approx 250M$ {cite}`rameshZeroShotTexttoImageGeneration2021`

Der CLIP Encoder wurde auf beiden trainiert. Prior, Decoder und Upsampler hingegen nur auf letzterem.

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
