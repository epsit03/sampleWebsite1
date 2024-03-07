---
layout: paper-note
title: "Quantized vs Distilled Neural Models: A Comparison"
description: "A dive into the techniques of quantizing and distilling deep learning models: What are they and how do they differ?"
date: 2023-11-1

bibliography: paper-notes.bib

---

Deep learning models, especially those with vast parameters, pose challenges for deployment in resource-constrained environments. Two popular techniques, quantization and distillation, address this issue, aiming to make these models more lightweight without compromising too much on performance. But what do they entail, and how do they compare?

<div class="l-page" style="text-align:center;">
  <img src="https://raw.githubusercontent.com/aadityaura/aadityaura.github.io/master/assets/img/Quantization.png" width="50%" style="margin-bottom: 12px; background-color: white;">
  <p style="text-align:center;">Quantization_vs_Distillation</p>
</div>


## Quantization: Precision for Efficiency

Quantization is all about numeric precision. By reducing the bit-width of weights and activations in a model, one can shrink the model size, potentially increasing inference speed.

**Math Behind Quantization:**

$$
\begin{aligned}
Q(r) &= \left\lfloor \frac{r}{S} \right\rfloor - Z \\
\text{where:} \\
Q & \text{is the quantization operator,} \\
r & \text{is a real-valued input (activation or weight),} \\
S & \text{is a real-valued scaling factor, and} \\
Z & \text{is an integer zero point.}
\end{aligned}
$$

This formula provides a straightforward and computationally efficient method for converting real numbers into quantized integers, making it a popular choice in many quantization schemes.

### Pros
- **Reduced Model Size:** Shifting from 32-bit floating points to 8-bit integers, for instance, can reduce the model size fourfold.
- **Speed and Hardware Compatibility:** Low precision arithmetic can be more rapid on specific hardware accelerators.
- **Memory Efficiency:** Less data means reduced memory bandwidth requirements.

### Cons
- **Accuracy Trade-offs:** Lower precision can sometimes affect model performance.
- **Implementation Challenges:** Quantization, particularly quantization-aware training, can be tricky.

## Distillation: From Teacher to Student

Distillation involves training a smaller neural network, called the student, to mimic a larger pre-trained network, the teacher.

**Math Behind Distillation:**

The objective in distillation is to minimize the divergence between the teacher's predictions and the student's predictions. The most commonly used measure for this divergence is the Kullback-Leibler divergence:

$$
\begin{aligned}
L &= \sum_i y_i^{(T)} \log\left(\frac{y_i^{(T)}}{y_i^{(S)}}\right) \\
y_i^{(T)} &\text{ is the output of the teacher model for class } i \\
y_i^{(S)} &\text{ is the output of the student model for class } i
\end{aligned}
$$

### Pros
- **Size Flexibility:** The student model's architecture or size can be customized, offering a balance between size and performance.
- **Performance Retention:** A well-distilled student can achieve performance close to its teacher, despite being more compact.

### Cons
- **Retraining is a Must:** Unlike quantization, distillation mandates retraining of the student model.
- **Training Overheads:** Time and computational resources are needed to train the student model.

## In Practice

Quantization often finds its place in hardware-specific deployments, while distillation is sought when one desires a lightweight model with performance close to a larger counterpart. In many scenarios, a combination of both – distilling a model and then quantizing it – can bring forth the benefits of both worlds. It's essential to align the choice with the deployment needs, available resources, and acceptable trade-offs in terms of accuracy and efficiency.
