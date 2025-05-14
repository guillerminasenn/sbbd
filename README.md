# sbbd
Scalable MCMC methods for Bayesian Blind Deconvolution

📦 Project Overview
This package implements MCMC methods for blind deconvolution with support for multiple algorithms, topologies (cyclic, Euclidean), and compute modes (traditional vs efficient gradient evaluation). The code is modular and extensible, designed for both method development and empirical testing under controlled synthetic data.
🧪 Simulated Data Generation
Synthetic data generation (aka "inverse crime" following Kaipio & Somersalo) is handled via the classes/ module:
The user instantiates a Lattice object to define the spatial domain.
Then a ConvolutionalModel object is instantiated using this lattice.
The model generates a reflectivity signal c, a wavelet w, and observed data d via convolution.
All model parameters, generated data, and metadata are stored within the ConvolutionalModel object.
🔁 MCMC Inference
The MCMC sampling is orchestrated through the MCMC class in mcmc.py:
The user creates an MCMC object by passing:
The observed data
Initial values for the latent variables
A model object (typically the same ConvolutionalModel) to inherit hyperparameters and topology
The user calls the .run() method on the MCMC object, passing:
The algorithm name (e.g., collapsed_hmc)
Number of iterations N
Algorithm-specific settings
⚙️ Internal Run Logic
Internally, the .run() method:
Dispatches to the appropriate run_<algorithm>.py script based on the chosen algorithm.
Uses topology_dispatch.py to route to the correct implementation depending on lattice topology (e.g., euclidean, cyclic).
Initializes all necessary sampler state, fixed quantities, and selects step_<algorithm>.py for iteration.
Within each iteration:
Algorithm-specific step logic is executed, based on the chosen compute mode (traditional vs efficient).
Adaptation logic (e.g., step size tuning) is handled via adapt.py.
Gradient and potential function calls are dynamically dispatched according to compute mode.
