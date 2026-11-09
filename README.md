# AI-Generated Image Checker

A small website that estimates whether an image is real or AI-generated and shows a
heatmap of the parts of the image the model focused on. Built with PyTorch.

## Why this project
AI-generated images are now easy to make and easy to share, and most people cannot
tell them apart from photos. A simple, honest checker can help people think twice
before trusting or forwarding an image. This project also measures how well such a
detector works on generators it has never seen, which is where many detectors fail.

## Dataset
MS COCOAI (Defactify Image Dataset), from Hugging Face:
https://huggingface.co/datasets/Rajarshi-Roy-research/Defactify_Image_Dataset

- 6,000 real photos (from MS COCO)
- 3,000 images from each of 5 generators: Stable Diffusion 2.1, SDXL, Stable Diffusion 3,
  DALL-E 3 and Midjourney v6

Please see the dataset page for its license and for how to cite its authors.

## Preprocessing
- Every image is center-cropped to a square, resized to 256x256 and saved as JPEG
  (quality 95).
- Real and fake images go through exactly the same steps, so the model cannot learn
  from differences in file format, shape or stretching.
- Original format and size are recorded in meta.csv for auditing.

## Train / validation / test split
- Train and validation: real images plus SD 2.1 and SDXL images.
- Test: held-out real, SD 2.1 and SDXL images, plus three generators the model never
  sees in training (SD 3, DALL-E 3, Midjourney v6).
- The split was fixed before any training. Results are reported per generator.

## Method (planned)
- Transfer learning with a pretrained ResNet, fine-tuned for real vs fake.
- Grad-CAM heatmaps to show what the model looked at.
- Confidence calibration, so the website can say "uncertain" when it is unsure.
- A small website built with FastAPI.

## Results
To be added after training and evaluation.

## Known limitations
- The data has one source of real photos (MS COCO) and five generators, so results
  describe this setup and not every fake image online.
- Different generators are shrunk by different amounts when resized to 256x256, so
  some resolution-related traces may remain.
- The checker gives a likelihood, not proof. It can be wrong.

## Project structure
    notebooks/   Colab notebooks (data preparation, later training)
    data/        small CSV files: meta, train, val, test (no images)

The images are not stored in this repo. They can be rebuilt with the notebook in
notebooks/.

## Status
- [x] Phase 1: data selection, preprocessing, split
- [ ] Phase 2: model training and evaluation on unseen generators
- [ ] Phase 3: explanations and calibration
- [ ] Phase 4: website
- [ ] Phase 5: testing and deployment
