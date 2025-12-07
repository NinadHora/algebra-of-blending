# algebra-of-blending
 A ground-up implementation of the **Poisson Image Editing** algorithm (Pérez et al., 2003), exploring the Linear Algebra behind gradient-domain image manipulation.
# Algebra of Blending: Poisson Image Editing

> Uma implementação "from scratch" do algoritmo de **Poisson Image Editing** (Pérez et al., 2003), explorando a Álgebra Linear por trás da manipulação de imagens no domínio do gradiente.

![Poisson Blending Demo]()
<img width="1090" height="431" alt="Screenshot 2025-12-06 at 21 26 07" src="https://github.com/user-attachments/assets/4169e3cb-f89e-4fc3-98a9-f5b8d12b0029" />

## Mathematical Theory

This project does not manipulate pixel intensities directly. Instead, it solves a **gradient optimization problem**.

The fundamental insight is that the human visual system is more sensitive to local contrast (gradients) than absolute luminance. To seamlessly clone an object ($g$) into a target background ($S$), we seek a new image function $f$ that:

1.  Preserves the **gradient field** of the source object ($\nabla f \approx \nabla g$) within the masking region $\Omega$.
2.  Respects the **Dirichlet boundary conditions** imposed by the target background ($f|_{\partial \Omega} = f^*|_{\partial \Omega}$).

Mathematically, we minimize the following variational problem:

$$\min_f \iint_{\Omega} |\nabla f - \mathbf{v}|^2 \quad \text{with} \quad f|_{\partial \Omega} = f^*|_{\partial \Omega}$$

This is equivalent to solving the **Poisson Partial Differential Equation**:

$$\Delta f = \text{div}(\mathbf{v}) \quad \text{over } \Omega$$

Where $\Delta$ is the Laplacian Operator ($\frac{\partial^2}{\partial x^2} + \frac{\partial^2}{\partial y^2}$).

### Numerical Solution (Discretization)

To solve this computationally, the continuous problem is discretized into a linear system of the form $Ax = b$:

* **$x$**: The vector of unknown pixel intensities inside the mask.
* **$A$**: A **Sparse Matrix** representing the Discrete Laplacian (using the standard 5-point stencil).
* **$b$**: The guidance vector containing the Laplacian of the source image + the intensities of the target image at the boundaries.

## Project Structure

The repository is structured as a progressive study, moving from 1D intuition to full 2D RGB blending.

```text
algebra-of-blending/
├── notebooks/
│   ├── 01_Poisson_1D_Toy_Problem.ipynb  # 1D Proof of concept (blending curves)
│   ├── 02_Matrix_Logic.ipynb            # Visualizing the Sparse Matrix A structure
│   └── 03_Image_Blending.ipynb          # Full algorithm applied to RGB channels
├── src/                                 # Modular scripts (WIP)
├── images/                              # Test assets (Source, Target, Mask)
└── README.md
````

## How to Run

This project relies on `numpy` for matrix algebra and `scipy.sparse` for efficiently solving large linear systems.

1.  Clone the repository:

    ```bash
    git clone [https://github.com/your-username/algebra-of-blending.git](https://github.com/your-username/algebra-of-blending.git)
    cd algebra-of-blending
    ```

2.  Install dependencies:

    ```bash
    pip install numpy scipy matplotlib opencv-python
    ```

3.  Run the notebooks in numerical order to follow the mathematical derivation.

## Insights & Limitations

During implementation, the following behaviors were observed and documented:

  * **Color Bleeding:** Since the algorithm enforces continuity at the boundaries, the color of the background often "seeps" into the pasted object. This is not a bug, but the mathematically correct solution to smooth the gradient field ($\Delta f$).
  * **Computational Complexity:** The matrix $A$ has dimensions $N \times N$ (where $N$ is the number of pixels in the mask). For HD images, dense storage is impossible ($O(N^2)$). Utilizing **Sparse Matrices** (LIL/CSR formats) reduces memory usage to $O(N)$, making the solution feasible on standard hardware.

## References

  * Pérez, P., Gangnet, M., & Blake, A. (2003). **Poisson Image Editing**. *ACM Transactions on Graphics (SIGGRAPH)*.
  * Strang, G. **Linear Algebra and Its Applications**. (For theory on Laplacian matrices).

-----

*Developed as a study project on Image Algebra and Computer Vision.*

```
`linear-algebra` `computer-vision` `poisson-equation` `image-processing` `scipy` `mathematics`
