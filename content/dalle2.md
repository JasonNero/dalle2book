# DALL·E-2 Presentation

## Text-2-Image

:::{note}
Definition of Text-2-Image and where it is used.
:::

## Previous Work

:::{mermaid}
gantt
    title Recent Publications for Text-2-Image Tasks
    dateFormat  YYYY-MM-DD
    axisFormat  %Y-%m
    todaymarker off
    section OpenAI
        %Improved Diffusion  :milestone, 2021-02-18,
        DALL·E              :milestone, 2021-02-24,
        CLIP                :milestone, 2021-02-26,
        Guided Diffusion (Diffusion Models Beat GANs on Image Synthesis) :milestone, 2021-05-11,
        GLIDE               :milestone, 2021-12-20,
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
        Parti               :milestone, 2022-06-22,
:::

### DALL·E

:::{note}
Nur kurz zeigen und sagen dass 2 nicht so recht auf 1 aufbaut.
:::

### CLIP

:::{note}
Was ist CLIP und wofuer wird es verwendet?
:::

### GLIDE

:::{figure} attachments/diffusion.gif
Illustration of Reverse Diffusion Process.
Source: [Alex Nichol](https://aqnichol.com/) {cite}`rameshHowDALLWorks`.
:::

:::{note}
Baut auf Guided Diffusion auf.
Was ist Diffusion?
:::

#### Denoising Diffusion Probabilistic Models (DDPM)

Diffusion Modelle sind Generative Modelle, sie generieren Daten aehnlich
zu den Trainingsdaten mit denen sie trainiert wurden.
Vorgestellt wurden sie von {cite:t}`hoDenoisingDiffusionProbabilistic2020`
mit Inspiration aus den "nonequilibrium thermodynamics".

Das Model korrumpiert zuerst die Daten durch Aufaddieren von
Gaussian Noise und wird dann darauf trainiert die Daten wiederherzustellen.
Gelernt wird also der Reverse Process oder auch Denoising Process, daher
auch der Name Denoising Diffusion Probabilistic Models (DDPM).
Sobald das Training abgeschlossen ist, kann man neue Daten generieren,
indem man einfach Rauschen auf das Model gibt.

Ein grosser Vorteil der Diffusion Modelle gegenüber GANs ist, dass
kein Adversarial Training erforderlich ist. Jedoch erfordern sie
längere Inferenzzeiten.

## OpenAI DALL·E-2

:::{figure} attachments/dalle2explained_polarbear.gif
DALL·E-2 creating an image of a polarbear playing bass.
Source: https://openai.com/dall-e-2/.
:::

:::{figure} attachments/dalle2explained_cat_inpainting.gif
DALL·E-2 replacing a dog with a cat via Inpainting.
Source: https://openai.com/dall-e-2/.
:::

:::{figure} attachments/dalle2explained_mona_inpainting.gif
DALL·E-2 giving Mona Lisa a mohawk via Inpainting.
Source: https://openai.com/dall-e-2/.
:::

:::{figure} attachments/dalle2explained_monkey_inpainting.gif
DALL·E-2 imagining a tax paying monkey with a funny hat via Inpainting.
Source: https://openai.com/dall-e-2/.
:::

:::{figure} attachments/dalle1vs2.png
"a painting of a fox sitting in a field at sunrise in the style of
Claude Monet”
Source: https://openai.com/dall-e-2/.
:::

### Capabilities

:::{note}
Examples and abilities going further than just text-2-image.
:::

### Architecture

#### Prior

#### Decoder

#### Upsampler

### Explorations of the Latent Space

### Limitations

:::{note}
Spatiale Zuordnungen etc.
:::

## Further Work

### Google Imagen

:::{note}
Show Benchmarks
:::

### Google Parti

:::{note}
Vergleich herstellen zu GLIDE und DALLE2
:::

