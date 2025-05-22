| Topic            | PDF Link                                                                                                                                     | Streamlit App                                                                                      | Colab Notebook                                                                                                                                           |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
|Loan-Eligibility (ML and ANN)    | <a href="" target="_parent"><img src="https://img.shields.io/badge/Open in PDF-%23FF0000.svg?style=flat-square&logo=adobe&logoColor=white"/></a> | <a href="https://ds-cheat-sheets-sklearn.streamlit.app/" target="_parent"><img src="https://static.streamlit.io/badges/streamlit_badge_black_white.svg"/></a> | <a href="https://colab.research.google.com/drive/1dwoSbQvSd178-vzuGfoh9bGegHSDcUPp?usp=sharing" target="_parent"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a> |
| Brain Tumor (CNN) | <a href="" target="_parent"><img src="https://img.shields.io/badge/Open in PDF-%23FF0000.svg?style=flat-square&logo=adobe&logoColor=white"/></a> | <a href="" target="_parent"><img src="https://static.streamlit.io/badges/streamlit_badge_black_white.svg"/></a> | <a href="https://colab.research.google.com/drive/1W3Ut6Rai4Y3so0yDGNBPVPJQKtzAbhy_?usp=sharing" target="_parent"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a> |




# Difference Between PyTorch and TensorFlow

| Feature                     | PyTorch                                         | TensorFlow                                    |
|-----------------------------|------------------------------------------------|-----------------------------------------------|
| **Developer**               | Facebook (Meta)                                 | Google                                        |
| **Release Year**            | 2016                                           | 2015                                          |
| **Programming Style**       | Dynamic computation graph (define-by-run)      | Static computation graph (define-and-run)     |
| **Execution Mode**          | Eager execution by default                       | Originally static graph, now supports eager   |
| **Debugging**               | Easy to debug with standard Python tools       | Harder to debug static graph, easier with eager execution in TF 2.x |
| **API Simplicity**          | More pythonic and intuitive                      | Initially complex, improved with `tf.keras`  |
| **Community Focus**         | Popular in research and academia                 | Popular in production and industry            |
| **Deployment & Production**| Limited official deployment tools, growing ecosystem (TorchServe) | Extensive deployment tools (TensorFlow Serving, TensorFlow Lite, TF.js) |
| **Model Export Format**     | TorchScript for deployment                        | SavedModel                                    |
| **GPU Support**             | Strong, seamless GPU acceleration                | Strong GPU and TPU support                     |
| **Ecosystem & Tools**       | Growing ecosystem, integrations with Python libraries | Mature ecosystem including TF Extended (TFX), TensorBoard, TensorFlow Hub |
| **Serialization Format**   | `.pt` or `.pth` files                             | Protocol Buffers (SavedModel format)          |
| **Popularity in Industry** | Widely adopted in research; gaining industry use | Widely adopted in industry and production     |
| **Learning Curve**          | Easier for Python users, very intuitive          | Steeper, but simplified with Keras API        |

---

## Summary

- **PyTorch** uses a **dynamic computation graph**, making it more intuitive and easier to debug, preferred by researchers and learners.
- **TensorFlow** originally used a **static computation graph**, optimizing performance and deployment, preferred for production and scalable applications.
- TensorFlow has a more mature ecosystem for deployment and tools, while PyTorch is catching up quickly.
- Both frameworks are powerful and capable; choice depends on your project needs, familiarity, and deployment goals.


https://nn-visualizer.netlify.app/v3

https://playground.tensorflow.org/#activation=relu&batchSize=10&dataset=circle&regDataset=reg-plane&learningRate=0.01&regularizationRate=0&noise=0&networkShape=4,3&seed=0.47139&showTestData=false&discretize=false&percTrainData=50&x=true&y=true&xTimesY=false&xSquared=false&ySquared=false&cosX=false&sinX=false&cosY=false&sinY=false&collectStats=false&problem=classification&initZero=false&hideText=false


https://poloclub.github.io/cnn-explainer/

https://adamharley.com/nn_vis/cnn/3d.html


https://deeplizard.com/resource/pavq7noze2

https://adamharley.com/nn_vis/

https://medium.com/@RaghavPrabhu/cnn-architectures-lenet-alexnet-vgg-googlenet-and-resnet-7c81c017b848

https://medium.com/@draj0718/convolutional-neural-networks-cnn-architectures-explained-716fb197b243
