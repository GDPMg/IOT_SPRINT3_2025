## 👥 Grupo

- **Guilherme Dal Posolo Matheus** — RM: 98694  
- **Caique Chagas** — RM: 551943  
- **Guilherme Faustino Vargas** — RM: 98278  
- **João Lucas Yudi Hedi Handa** — RM: 98458  
- **Ryan Perez Pacheco** — RM: 98782  


# Face ID Local (OpenCV + Haar + LBPH)

## Objetivo
Autenticar/identificar usuário **100% offline** em desktop/notebook usando OpenCV.  
Pipeline: **detecção** (Haar Cascade) → **recorte** com margem → **reconhecimento** (LBPH).

## Arquitetura
- **Detecção:** `haarcascade_frontalface_default.xml` + `haarcascade_profileface.xml` (multi-ângulo).
- **Reconhecimento:** LBPH (`lbph.yml` + `labels.json`).
- **Pré-processamento opcional:** CLAHE, redimensionamento consistente.
- **Desempenho:** `FRAME_SKIP`, `resize`, parâmetros da detecção.

## Requisitos
- Python 3.10+  
- `pip install -r requirements.txt`
  - `opencv-contrib-python` (necessário para `cv2.face`)
  - `numpy`

## Estrutura de pastas

- face_app/
- collect.py
- train.py
- recognize.py
- haar/      -> arquivos .xml (Haar)
- data/      -> recortes por usuário (gerado)
- models/    -> lbph.yml e labels.json (gerado)
- videos/    -> seus vídeos de treino/teste

## ⚡ Ajustes rápidos
- **THRESHOLD (LBPH):** menor = mais rígido. Teste valores entre **70–110**.  
- **scaleFactor, minNeighbors, minSize (detecção).**  
- **FRAME_SKIP, resize** para performance.  
- **EXPAND** para pegar mais cabeça/cabelo.  

---

## 📦 Dependências

- [Python 3.10+](https://www.python.org/downloads/)
- [opencv-contrib-python](https://pypi.org/project/opencv-contrib-python/) → necessário para usar `cv2.face`
- [numpy](https://pypi.org/project/numpy/)

### Instalação
```bash
pip install opencv-contrib-python numpy
````

## 📊 Principais parâmetros e impacto

| Parâmetro        | Onde           | Efeito prático |
|------------------|----------------|----------------|
| `scaleFactor`    | detecção       | 1.1 mais sensível (lento), 1.3 mais rápido (menos sensível) |
| `minNeighbors`   | detecção       | ↑ reduz falso positivo, pode perder detecções fracas |
| `minSize`        | detecção       | ignora faces pequenas; ↑ acelera em vídeos de perto |
| `EXPAND`         | recorte        | ↑ pega mais da cabeça; ajuda quando vira de perfil |
| `THRESHOLD`      | LBPH           | ↑ diminui falso negativo; cuidado com falso aceite |
| `FRAME_SKIP`     | laço principal | pular frames = mais FPS |
| `resize` do frame| pré-process    | menor resolução = detecção mais rápida |
| `CLAHE`          | pré-process    | melhora contraste em luz ruim |

---

## ⚠️ Limitações
- Virações fortes de cabeça e oclusões (boné, máscara) prejudicam.  
- Luz extrema/contraluz atrapalha.  
- Dependência de detecção (Haar) para iniciar o recorte.  

---

## 🛠️ Troubleshooting
- **`AttributeError: cv2.face`** → instale `opencv-contrib-python`.  
- **“Salvei 0 recortes”** → verifique caminho do Haar XML (`haar/...xml`).  
- **Reconhece pouco** → ajuste `THRESHOLD`, use `EXPAND`, garanta mesma resolução treino/teste, reforce dataset (150–300 recortes variados).  
- **Vídeo lento** → use `FRAME_SKIP=2` e `resize` para 640×360.  

---

## 📜 Nota ética (uso de dados faciais)
- Este projeto roda **localmente** e não envia dados para terceiros.  
- Utilize apenas com **consentimento explícito** das pessoas filmadas.  
- Evite armazenar vídeos/recortes desnecessários; prefira **descartar após o treino**.  
- Não utilize para **discriminação**, **vigilância abusiva** ou fins que violem leis aplicáveis (ex.: LGPD).  
