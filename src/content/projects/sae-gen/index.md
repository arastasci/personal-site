---
title: "Style-Aligned Efficient 3D Model Generation"
description: "A pipeline for generating lightweight, style-consistent 3D models from a text prompt and reference image."
date: "Jan 15 2025"
demoURL: "https://github.com/arastasci/style-aligned-3d-model-gen"  # You can replace or remove this if no live demo
repoURL: "https://github.com/arastasci/style-aligned-3d-model-gen"
---

![Pipeline Diagram](/diagram7.png)

This project presents a novel pipeline for generating **style-aligned, efficient 3D models** from a text prompt and a reference image. It leverages three cutting-edge models:

- **Style-Aligned** for generating images that match a reference style
- **InstantMesh** for creating dense 3D meshes from images
- **MeshAnything** for generating artist-created, simplified meshes suitable for real-time use

The pipeline is designed to overcome two major problems in 3D generation: poor stylistic alignment and overly dense meshes.

## 🧠 Motivation

Modern text-to-3D systems are powerful but often fail in two key areas:
- They can’t consistently adhere to a **specific artistic style**.
- They generate **high-poly meshes** that aren’t suitable for games or interactive experiences.

This pipeline tackles both issues with modular components that work together to produce usable, stylized meshes.

## 🛠 Pipeline Overview

1. **Style-Aligned Image Generation**  
   Given a text prompt and style image, the Style-Aligned model outputs a reference image that reflects both the content and desired style using a shared-attention mechanism.

   ![Style-Aligned Diagram](/stylealignedshared.png)

2. **3D Mesh Generation with InstantMesh**  
   The image is fed to InstantMesh, which uses multi-view diffusion and a ViT-based triplane decoder to create a dense 3D mesh.

   ![InstantMesh Architecture](/instantmesh.png)

3. **Mesh Simplification with MeshAnything**  
   The dense mesh is simplified using MeshAnything to generate an **Artist-Created Mesh (AM)**. MeshAnything is trained to preserve structure while reducing polygon count.

   ![MeshAnything Process](/meshanythingfigureonlyu.png)

4. **Texture Transfer**  
   Since MeshAnything does not output textures or colors, vertex colors from the dense mesh are transferred to the simplified mesh using:
   - **ICP alignment**
   - **UV parameterization (trivial-per-triangle)**
   - **Barycentric interpolation in PyMeshLab**

## 📊 Results

![Pipeline Output Results](/results4.png)

- **Mesh complexity reduction**: ~98.43%
- **User-rated visual fidelity**: 7.8 / 10 (from a user study)
- Final meshes are performant enough for real-time use while maintaining stylistic accuracy.

## 💡 Challenges

- **Content shift & Janus-face** effects from diffusion-based stages
- **MeshAnything limitations** on detail (800-face max); potential future use of MeshAnythingV2

## ✅ Conclusion

This project demonstrates a proof-of-concept pipeline for generating stylized, low-poly 3D models with practical use in game development and real-time rendering. As generative 3D technologies advance, this modular pipeline can evolve further by swapping components with more powerful successors.
