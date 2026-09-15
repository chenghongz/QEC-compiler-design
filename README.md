## Contents

- [Logical-level QEC optimization](#s1)
  - [Clifford Operation Implementation](#s1-1)
    - [General Clifford and Lattice-Surgery Primitives](#s1-1-1)
    - [Lattice Surgery Introduction](#s1-1-2)
    - [Surface Code Lattice Surgery Compilers](#s1-1-3)
    - [Beyond Surface-Code and qLDPC Code-Surgery Compilers](#s1-1-4)
  - [Non-Clifford Operation Implementation](#s1-2)
    - [Magic State Distillation](#s1-2-1)
    - [Code Switching](#s1-2-2)
    - [Early Fault-Tolerant Compiler](#s1-2-3)
- [Physical-level QEC Implementation](#s2)
  - [Code-Level Syndrome-Extraction Scheduling](#s2-1)
  - [Hardware-Aware QEC Realization on Superconducting Circuits](#s2-2)
  - [Hardware-Aware QEC Realization on Trapped-Ion](#s2-3)
  - [Hardware-Aware QEC Realization on Neutral Atom Arrays](#s2-4)
- [Decoding in the Classical Control Stack](#s3)
  - [Algorithms and Implementation Trade-offs](#s3-1)
    - [Algorithm Overview and Tools](#s3-1-1)
    - [Matching / MWPM](#s3-1-2)
    - [Search and Hypergraph Decoding](#s3-1-3)
    - [Union-Find](#s3-1-4)
    - [Belief Propagation](#s3-1-5)
    - [Neural-Network Decoding](#s3-1-6)
  - [Real-Time Decoder Integration](#s3-2)
    - [Runtime Integration Overview](#s3-2-1)
    - [Pauli and Clifford Frames](#s3-2-2)
    - [Window and Speculative Decoding](#s3-2-3)
    - [Noise- and Side-Information-Aware Decoding](#s3-2-4)
- [Conclusion and Future Direction](#s4)
  - [Future Directions](#s4-1)

<a id="s1"></a>
## Logical-level QEC optimization

<a id="s1-1"></a>
### Clifford Operation Implementation

<a id="s1-1-1"></a>
#### General Clifford and Lattice-Surgery Primitives

- **[Surface codes: Towards practical large-scale quantum computation](https://doi.org/10.1103/physreva.86.032324)**
  Austin G Fowler, Matteo Mariantoni, John M Martinis, and Andrew N Cleland. *Physical Review A—Atomic, Molecular, and Optical Physics*, 2012.
- **[Low overhead quantum computation using lattice surgery](https://arxiv.org/abs/1808.06709)**
  Austin G Fowler, and Craig Gidney. *arXiv preprint arXiv:1808.06709*, 2018.

<a id="s1-1-2"></a>
#### Lattice Surgery Introduction

- **[Surface code quantum computing by lattice surgery](https://doi.org/10.1088/1367-2630/14/12/123011)**
  Dominic Horsman, Austin G Fowler, Simon Devitt, and Rodney Van Meter. *New Journal of Physics*, 2012.

<a id="s1-1-3"></a>
#### Surface Code Lattice Surgery Compilers

- **[Mapping of lattice surgery-based quantum circuits on surface code architectures](https://doi.org/10.1088/2058-9565/aadd1a)**
  Lingling Lao, Bas van Wee, Imran Ashraf, J Van Someren, Nader Khammassi, Koen Bertels, and Carmen G Almudever. *Quantum Science and Technology*, 2018.
- **[Optimization of lattice surgery is NP-hard](https://doi.org/10.1038/s41534-017-0035-1)**
  Daniel Herr, Franco Nori, and Simon J Devitt. *Npj quantum information*, 2017.
- **[A SAT Scalpel for Lattice Surgery: Representation and Synthesis of Subroutines for Surface-Code Fault-Tolerant Quantum Computing](https://doi.org/10.1109/isca59077.2024.00032)**
  Daniel Bochen Tan, Murphy Yuezhen Niu, and Craig Gidney. *Proceedings of the ACM/IEEE International Symposium on Computer Architecture (ISCA '24)*, 2024.
- **[Design automation and space-time reduction for surface-code logical operations using a SAT-based EDA kernel compatible with general encodings](https://arxiv.org/abs/2604.12560)**
  Wang Liao, Rei Tokami, and Yasunari Suzuki. *arXiv preprint arXiv:2604.12560*, 2026.
- **[Dependency-aware compilation for surface code quantum architectures](https://doi.org/10.1145/3720416)**
  Abtin Molavi, Amanda Xu, Swamit Tannu, and Aws Albarghouthi. *Proceedings of the ACM on Programming Languages*, 2025.
- **[A game of surface codes: Large-scale quantum computing with lattice surgery](https://doi.org/10.22331/q-2019-03-05-128)**
  Daniel Litinski. *Quantum*, 2019.
- **[Assessing requirements to scale to practical quantum advantage](https://arxiv.org/abs/2211.07629)**
  Michael E Beverland, Prakash Murali, Matthias Troyer, Krysta M Svore, Torsten Hoefler, Vadym Kliuchnikov, Guang Hao Low, Mathias Soeken, et al. *arXiv preprint arXiv:2211.07629*, 2022.
- **[Locality-Aware Pauli-based Computation for Local Magic State Preparation](https://arxiv.org/abs/2504.12091)**
  Yutaka Hirano, and Keisuke Fujii. *arXiv preprint arXiv:2504.12091*, 2025.
- **[Multi-qubit lattice surgery scheduling](https://arxiv.org/abs/2405.17688)**
  Allyson Silva, Xiangyi Zhang, Zak Webb, Mia Kramer, Chan Woo Yang, Xiao Liu, Jessica Lemieux, Ka-Wai Chen, et al. *arXiv preprint arXiv:2405.17688*, 2024.
- **[A Substrate Scheduler for Compiling Arbitrary Fault-Tolerant Graph States](https://doi.org/10.1109/qce57702.2023.00101)**
  Sitong Liu, Naphan Benchasattabuse, Darcy QC Morgan, Michal Hajdu sek, Simon J. Devitt, and Rodney Van Meter. *Proceedings of the IEEE International Conference on Quantum Computing and Engineering (QCE '23)*, 2023.
- **[A High Performance Compiler for Very Large Scale Surface Code Computations](https://doi.org/10.22331/q-2024-05-22-1354)**
  George Watkins, Hoang Minh Nguyen, Keelan Watkins, Steven Pearce, Hoi-Kwan Lau, and Alexandru Paler. *Quantum*, 2024.
- **[Transpiler-Architecture Co-Design to Curb Clifford Costs in Fault-Tolerant Quantum Computing](https://arxiv.org/abs/2412.15434)**
  Meng Wang, Chenxu Liu, Samuel Stein, Yufei Ding, Poulami Das, Prashant J. Nair, and Ang Li. *arXiv preprint arXiv:2412.15434*, 2024.
- **[Tableau-Based Framework for Efficient Logical Quantum Compilation](https://arxiv.org/abs/2509.02721)**
  Meng Wang, Chenxu Liu, Sean Garner, Samuel Stein, Yufei Ding, Prashant J Nair, and Ang Li. *arXiv preprint arXiv:2509.02721*, 2025.
- **[Ecmas: Efficient circuit mapping and scheduling for surface code](https://doi.org/10.1109/cgo57630.2024.10444874)**
  Mingzheng Zhu, Hao Fu, Jun Wu, Chi Zhang, Wei Xie, and Xiang-Yang Li. *Proceedings of the IEEE/ACM International Symposium on Code Generation and Optimization (CGO '24)*, 2024.
- **[Surface Code Compilation via Edge-Disjoint Paths](https://doi.org/10.1103/prxquantum.3.020342)**
  Michael Beverland, Vadym Kliuchnikov, and Eddie Schoute. *PRX Quantum*, 2022.
- **[Trace-Based Reconstruction of Quantum Circuit Dataflow in Surface Codes](https://arxiv.org/abs/2508.14533)**
  Theodoros Trochatos, Christopher Kang, Andrew Wang, Frederic T Chong, and Jakub Szefer. *arXiv preprint arXiv:2508.14533*, 2025.
- **[Efficient and High-Performance Routing of Lattice-Surgery Paths on Three-Dimensional Lattice](https://arxiv.org/abs/2401.15829)**
  Kou Hamada, Yasunari Suzuki, and Yuuki Tokunaga. *arXiv preprint arXiv:2401.15829*, 2024.
- **[TopoLS: Lattice Surgery Compilation via Topological Program Transformations](https://arxiv.org/abs/2601.23109)**
  Junyu Zhou, Yuhao Liu, Ethan Decker, Justin Kalloor, Mathias Weiden, Kean Chen, Costin Iancu, and Gushu Li. *arXiv preprint arXiv:2601.23109*, 2026.
- **[O3LS: Optimizing Lattice Surgery via Automatic Layout Searching and Loose Scheduling](https://arxiv.org/abs/2604.15099)**
  Chenghong Zhu, Xian Wu, Jiahan Chen, Keming He, Junjie Wu, Xin Wang, and Lingling Lao. *arXiv preprint arXiv:2604.15099*, 2026.
- **[High-Performance and Scalable Fault-Tolerant Quantum Computation with Lattice Surgery on a 2.5D Architecture](https://arxiv.org/abs/2411.17519)**
  Yosuke Ueno, Taku Saito, Teruo Tanimoto, Yasunari Suzuki, Yutaka Tabuchi, Shuhei Tamate, and Hiroshi Nakamura. *arXiv preprint arXiv:2411.17519*, 2024.
- **[SPARO: Surface-code Pauli-Based Architectural Resource Optimization for Fault-Tolerant Quantum Computing](https://arxiv.org/abs/2504.21854)**
  Shuwen Kan, Zefan Du, Chenxu Liu, Meng Wang, Yufei Ding, Ang Li, Ying Mao, and Samuel Stein. *arXiv preprint arXiv:2504.21854*, 2025.
- **[The Q-Spellbook: Crafting Surface Code Layouts and Magic State Protocols for Large-Scale Quantum Computing](https://arxiv.org/abs/2502.11253)**
  Avimita Chatterjee, Archisman Ghosh, and Swaroop Ghosh. *arXiv preprint arXiv:2502.11253*, 2025.
- **[Compilation of trotter-based time evolution for partially fault-tolerant quantum computing architecture](https://doi.org/10.1103/93zr-1ykb)**
  Yutaro Akahoshi, Riki Toshio, Jun Fujisaki, Hirotaka Oshima, Shintaro Sato, and Keisuke Fujii. *PRX Quantum*, 2025.
- **[RESCQ: Realtime Scheduling for Continuous Angle Quantum Error Correction Architectures](https://doi.org/10.1145/3676641.3716018)**
  Sayam Sethi, and Jonathan Mark Baker. *Proceedings of the ACM International Conference on Architectural Support for Programming Languages and Operating Systems (ASPLOS '25)*, 2025.
- **[Variational quantum algorithms in the era of early fault tolerance](https://doi.org/10.1145/3695053.3731112)**
  Siddharth Dangwal, Suhas Vittal, Lennart Maximilian Seifert, Frederic T Chong, and Gokul Subramanian Ravi. *Proceedings of the ACM/IEEE International Symposium on Computer Architecture (ISCA '25)*, 2025.
- **[The Solovay-Kitaev algorithm](https://doi.org/10.26421/qic6.1-6)**
  Christopher M. Dawson, and Michael A. Nielsen. *Quantum Information and Computation*, 2006.
- **[Optimal ancilla-free Clifford+T approximation of Z-rotations](https://doi.org/10.26421/qic16.11-12-1)**
  Neil J. Ross, and Peter Selinger. *Quantum Information and Computation*, 2016.
- **[Realistic Cost to Execute Practical Quantum Circuits using Direct Clifford+ T Lattice Surgery Compilation](https://doi.org/10.1145/3689826)**
  Tyler LeBlond, Christopher Dean, George Watkins, and Ryan Bennink. *ACM Transactions on Quantum Computing*, 2023.
- **[Lattice Surgery Compiler](https://github.com/latticesurgery-com/lattice-surgery-compiler)**
  latticesurgery-com. 2025.
- **[Ecmas+: Efficient Circuit Mapping and Scheduling for Surface Code Encoded Circuit on Quantum Cloud Platform](https://doi.org/10.1145/3760783)**
  Mingzheng Zhu, Hao Fu, Haishan Song, Jun Wu, Chi Zhang, Wei Xie, and Xiangyang Li. *ACM Transactions on Architecture and Code Optimization*, 2025.
- **[PureMagic: A Dynamic Scheduler for Lattice Surgery](https://arxiv.org/abs/2512.06484)**
  Steven Hofmeyr, Mathias Weiden, Justin Kalloor, John Kubiatowicz, and Costin Iancu. *arXiv preprint arXiv:2512.06484*, 2026.
- **[Non-Clifford Fusion: T-Gate Optimization for Quantum Simulation](https://arxiv.org/abs/2510.13573)**
  Yingheng Li, Xulong Tang, Paul Hovland, and Ji Liu. 2025.

<a id="s1-1-4"></a>
#### Beyond Surface-Code and qLDPC Code-Surgery Compilers

- **[Lattice Surgery Compilation beyond the Surface Code](https://arxiv.org/abs/2504.10591)**
  Laura S. Herzog, Lucas Berent, Aleksander Kubica, and Robert Wille. *arXiv preprint arXiv:2504.10591*, 2025.
- **[SSIP: automated surgery with quantum LDPC codes](https://arxiv.org/abs/2407.09423)**
  Alexander Cowtan. *arXiv preprint arXiv:2407.09423*, 2024.
- **[Engineering CSS surgery: compiling any CNOT in any code](https://arxiv.org/abs/2505.01370)**
  Clement Poirson, Joschka Roffe, and Robert I Booth. *arXiv preprint arXiv:2505.01370*, 2025.
- **[GeneCS: Synthesizing Resource-Efficient Code Surgery for Arbitrary Quantum Stabilizer Codes](https://arxiv.org/abs/2605.21746)**
  Junyu Zhou, Ali Javadi-Abhari, and Gushu Li. *arXiv preprint arXiv:2605.21746*, 2026.
- **[Extractors: QLDPC Architectures for Efficient Pauli-Based Computation](https://arxiv.org/abs/2503.10390)**
  Zhiyang He, Alexander Cowtan, Dominic J Williamson, and Theodore J Yoder. *arXiv preprint arXiv:2503.10390*, 2025.
- **[Tour de gross: A modular quantum computer based on bivariate bicycle codes](https://arxiv.org/abs/2506.03094)**
  Theodore J Yoder, Eddie Schoute, Patrick Rall, Emily Pritchett, Jay M Gambetta, Andrew W Cross, Malcolm Carroll, and Michael E Beverland. *arXiv preprint arXiv:2506.03094*, 2025.
- **[Assessing System Capabilities and Bottlenecks of an Early Fault-Tolerant Bicycle Architecture](https://arxiv.org/abs/2604.20013)**
  Kun Liu, Ben Foxman, Gian-Luca R Anselmetti, and Yongshan Ding. *arXiv preprint arXiv:2604.20013*, 2026.
- **[Low-overhead fault-tolerant quantum computing using long-range connectivity](https://doi.org/10.1126/sciadv.abn1717)**
  Lawrence Z Cohen, Isaac H Kim, Stephen D Bartlett, and Benjamin J Brown. *Science Advances*, 2022.
- **[Improved QLDPC surgery: Logical measurements and bridging codes](https://arxiv.org/abs/2407.18393)**
  Andrew W Cross, Zhiyang He, Patrick J Rall, and Theodore J Yoder. *arXiv preprint arXiv:2407.18393*, 2024.
- **[Time-efficient logical operations on quantum low-density parity check codes](https://doi.org/10.1103/physrevlett.134.070602)**
  Guo Zhang, and Ying Li. *Physical Review Letters*, 2025.
- **[Low-overhead fault-tolerant quantum computation by gauging logical operators](https://doi.org/10.1038/s41567-026-03220-8)**
  Dominic J Williamson, and Theodore J Yoder. *Nature Physics*, 2026.
- **[Fault-tolerant logical measurements via homological measurement](https://doi.org/10.1103/physrevx.15.021088)**
  Benjamin Ide, Manoj G Gowda, Priya J Nadkarni, and Guillaume Dauphinais. *Physical Review X*, 2025.
- **[Universal adapters between quantum low-density parity check codes](https://doi.org/10.1103/1g44-jp62)**
  Esha Swaroop, Tomas Jochym-O'Connor, and Theodore J Yoder. *PRX Quantum*, 2026.
- **[Parallel logical measurements via quantum code surgery](https://doi.org/10.1103/gj8x-n5gg)**
  Alexander Cowtan, Zhiyang He, Dominic J Williamson, and Theodore J Yoder. *PRX Quantum*, 2026.
- **[Fast surgery for quantum LDPC codes](https://arxiv.org/abs/2510.04521)**
  Nouedyn Baspin, Lucas Berent, and Lawrence Z Cohen. *arXiv preprint arXiv:2510.04521*, 2025.
- **[Parsimonious Quantum Low-Density Parity-Check Code Surgery](https://arxiv.org/abs/2603.05082)**
  Andrew C Yuan, Alexander Cowtan, Zhiyang He, Ting-Chun Lin, and Dominic J Williamson. *arXiv preprint arXiv:2603.05082*, 2026.
- **[Batched high-rate logical operations for quantum LDPC codes](https://arxiv.org/abs/2510.06159)**
  Qian Xu, Hengyun Zhou, Dolev Bluvstein, Madelyn Cain, Marcin Kalinowski, John Preskill, Mikhail D Lukin, and Nishad Maskara. *arXiv preprint arXiv:2510.06159*, 2025.

<a id="s1-2"></a>
### Non-Clifford Operation Implementation

<a id="s1-2-1"></a>
#### Magic State Distillation

- **[Topological fault-tolerance in cluster state quantum computation](https://doi.org/10.1088/1367-2630/9/6/199)**
  Robert Raussendorf, Jim Harrington, and Kovid Goyal. *New Journal of Physics*, 2007.
- **[A fault-tolerant one-way quantum computer](https://doi.org/10.1016/j.aop.2006.01.012)**
  Robert Raussendorf, Jim Harrington, and Kovid Goyal. *Annals of physics*, 2006.
- **[Topological quantum distillation](https://doi.org/10.1103/physrevlett.97.180501)**
  Hector Bombin, and Miguel Angel Martin-Delgado. *Physical review letters*, 2006.
- **[Magic-state distillation with low overhead](https://doi.org/10.1103/physreva.86.052329)**
  Sergey Bravyi, and Jeongwan Haah. *Physical Review A—Atomic, Molecular, and Optical Physics*, 2012.
- **[Magic state distillation with low space overhead and optimal asymptotic input count](https://doi.org/10.22331/q-2017-10-03-31)**
  Jeongwan Haah, Matthew B Hastings, David Poulin, and D Wecker. *Quantum*, 2017.
- **[Codes and Protocols for Distilling T, Controlled-S, and Toffoli Gates](https://doi.org/10.22331/q-2018-06-07-71)**
  Jeongwan Haah, and Matthew B Hastings. *Quantum*, 2018.
- **[Constant-overhead magic state distillation](https://doi.org/10.1038/s41567-025-03026-0)**
  Adam Wills, Min-Hsiu Hsieh, and Hayata Yamasaki. *Nature Physics*, 2025.
- **[Efficient Magic State Distillation by Zero-Level Distillation](https://doi.org/10.1103/thxx-njr6)**
  Tomohiro Itogawa, Yugo Takada, Yutaka Hirano, and Keisuke Fujii. *PRX Quantum*, 2025.
- **[Very low overhead fault-tolerant magic state preparation using redundant ancilla encoding and flag qubits](https://doi.org/10.1038/s41534-020-00319-5)**
  Christopher Chamberland, and Kyungjoo Noh. *npj Quantum Information*, 2020.
- **[Magic state cultivation: growing T states as cheap as CNOT gates](https://arxiv.org/abs/2409.17595)**
  Craig Gidney, Noah Shutty, and Cody Jones. *arXiv preprint arXiv:2409.17595*, 2024.
- **[Efficient magic state cultivation with lattice surgery](https://arxiv.org/abs/2510.24615)**
  Yutaka Hirano, Riki Toshio, Tomohiro Itogawa, and Keisuke Fujii. *arXiv preprint arXiv:2510.24615*, 2025.
- **[Magic State Cultivation on the Surface Code](https://arxiv.org/abs/2502.01743)**
  Yotam Vaknin, Shoham Jacoby, Arne Grimsmo, and Alex Retzker. *arXiv preprint arXiv:2502.01743*, 2025.
- **[Cultivating T states on the surface code with only two-qubit gates](https://arxiv.org/abs/2509.05232)**
  Jahan Claes. *arXiv preprint arXiv:2509.05232*, 2025.
- **[Fold-Transversal Surface Code Cultivation](https://doi.org/10.1103/gpvl-lg4c)**
  Kaavya Sahay, Pei-Kai Tsai, Kathleen Chang, Qile Su, Thomas B Smith, Shraddha Singh, and Shruti Puri. *PRX Quantum*, 2026.
- **[Magic-state functional units: Mapping and scheduling multi-level distillation circuits for fault-tolerant quantum architectures](https://doi.org/10.1109/micro.2018.00072)**
  Yongshan Ding, Adam Holmes, Ali Javadi-Abhari, Diana Franklin, Margaret Martonosi, and Frederic Chong. *Proceedings of the IEEE/ACM International Symposium on Microarchitecture (MICRO '18)*, 2018.
- **[Resource optimized quantum architectures for surface code implementations of magic-state distillation](https://doi.org/10.1016/j.micpro.2019.02.007)**
  Adam Holmes, Yongshan Ding, Ali Javadi-Abhari, Diana Franklin, Margaret Martonosi, and Frederic T Chong. *Microprocessors and Microsystems*, 2019.
- **[Magic state distillation: Not as costly as you think](https://doi.org/10.22331/q-2019-12-02-205)**
  Daniel Litinski. *Quantum*, 2019.
- **[Leveraging zero-level distillation to generate high-fidelity magic states](https://doi.org/10.1109/qce60285.2024.00104)**
  Yutaka Hirano, Tomohiro Itogawa, and Keisuke Fujii. *Proceedings of the IEEE International Conference on Quantum Computing and Engineering (QCE '24)*, 2024.
- **[MagicPool: Dealing with Magic State Distillation Failures on Large-Scale Fault-Tolerant Quantum Computer](https://arxiv.org/abs/2407.07394)**
  Yutaka Hirano, Yasunari Suzuki, and Keisuke Fujii. *arXiv preprint arXiv:2407.07394*, 2024.
- **[Universal quantum computation with ideal Clifford gates and noisy ancillas](https://doi.org/10.1103/physreva.71.022316)**
  Sergey Bravyi, and Alexei Kitaev. *Physical Review A*, 2005.

<a id="s1-2-2"></a>
#### Code Switching

- **[Minimizing the Number of Code Switching Operations in Fault-Tolerant Quantum Circuits](https://arxiv.org/abs/2512.04170)**
  Erik Weilandt, Tom Peham, and Robert Wille. *arXiv preprint arXiv:2512.04170*, 2025.
- **[Enabling Full-Stack Quantum Computing with Changeable Error-Corrected Qubits](https://arxiv.org/abs/2305.07072)**
  Anbang Wu, Keyi Yin, Andrew W. Cross, Ang Li, and Yufei Ding. *arXiv preprint arXiv:2305.07072*, 2023.
- **[Architectures for Heterogeneous Quantum Error Correction Codes](https://arxiv.org/abs/2411.03202)**
  Samuel Stein, Shifan Xu, Andrew W. Cross, Theodore J. Yoder, Ali Javadi-Abhari, Chenxu Liu, Kun Liu, Zeyuan Zhou, et al. *arXiv preprint arXiv:2411.03202*, 2024.

<a id="s1-2-3"></a>
#### Early Fault-Tolerant Compiler

- **[Partially Fault-Tolerant Quantum Computing Architecture with Error-Corrected Clifford Gates and Space-Time Efficient Analog Rotations](https://doi.org/10.1103/prxquantum.5.010337)**
  Yutaro Akahoshi, Kazunori Maruyama, Hirotaka Oshima, Shintaro Sato, and Keisuke Fujii. *PRX Quantum*, 2024.
- **[Fast and Parallel High-Rate STAR Architecture for Megaquop Quantum Simulation](https://arxiv.org/abs/2606.25011)**
  Refaat Ismail, Milan Kornjača, Hong-Ye Hu, Nishad Maskara, Sheng-Tao Wang, Hengyun Zhou, and Chen Zhao. 2026.

<a id="s2"></a>
## Physical-level QEC Implementation

<a id="s2-1"></a>
### Code-Level Syndrome-Extraction Scheduling

- **[Realization of Real-Time Fault-Tolerant Quantum Error Correction](https://doi.org/10.1103/physrevx.11.041058)**
  C. Ryan-Anderson, J. G. Bohnet, K. Lee, D. Gresh, A. Hankin, J. P. Gaebler, D. Francois, A. Chernoguzov, et al. *Physical Review X*, 2021.
- **[Realization of an error-correcting surface code with superconducting qubits](https://doi.org/10.1103/physrevlett.129.030501)**
  Youwei Zhao, Yangsen Ye, He-Liang Huang, Yiming Zhang, Dachao Wu, Huijie Guan, Qingling Zhu, Zuolin Wei, et al. *Physical Review Letters*, 2022.
- **[QUITS: A modular Qldpc code circUIT Simulator](https://doi.org/10.22331/q-2025-12-05-1931)**
  Mingyu Kang, Yingjia Lin, Hanwen Yao, Mert Gokduman, Arianna Meinking, and Kenneth R Brown. *Quantum*, 2025.
- **[Magic Tricycles: Efficient Magic-State Generation with Finite Block-Length Quantum LDPC Codes](https://doi.org/10.1103/ghhp-cytl)**
  Varun Menon, J Pablo Bonilla Ataides, Rohan Mehta, Andi Gu, Daniel Bochen Tan, and Mikhail D Lukin. *Physical Review X*, 2026.
- **[Surface code off-the-hook: diagonal syndrome-extraction scheduling](https://arxiv.org/abs/2602.09099)**
  Gilad Kishony, and Austin Fowler. *arXiv preprint arXiv:2602.09099*, 2026.
- **[High-threshold and low-overhead fault-tolerant quantum memory](https://doi.org/10.1038/s41586-024-07107-7)**
  Sergey Bravyi, Andrew W Cross, Jay M Gambetta, Dmitri Maslov, Patrick Rall, and Theodore J Yoder. *Nature*, 2024.
- **[Tangling schedules eases hardware connectivity requirements for quantum error correction](https://doi.org/10.1103/prxquantum.5.010348)**
  Gyorgy P Geher, Ophelia Crawford, and Earl T Campbell. *PRX Quantum*, 2024.
- **[Distance-preserving stabilizer measurements in hypergraph product codes](https://doi.org/10.22331/q-2025-01-30-1618)**
  Argyris Giannisis Manes, and Jahan Claes. *Quantum*, 2025.
- **[AlphaSyndrome: Tackling the syndrome measurement circuit scheduling problem for QEC codes](https://doi.org/10.1145/3779212.3790123)**
  Yuhao Liu, Shuohao Ping, Junyu Zhou, Ethan Decker, Justin Kalloor, Mathias Weiden, Kean Chen, Yunong Shi, et al. *Proceedings of the ACM International Conference on Architectural Support for Programming Languages and Operating Systems (ASPLOS '26)*, 2026.
- **[PropHunt: Automated optimization of quantum syndrome measurement circuits](https://doi.org/10.1145/3779212.3790205)**
  Joshua Viszlai, Satvik Maurya, Swamit Tannu, Margaret Martonosi, and Frederic T Chong. *Proceedings of the ACM International Conference on Architectural Support for Programming Languages and Operating Systems (ASPLOS '26)*, 2026.
- **[Optimal Compilation of Syndrome Extraction Circuits for General Quantum LDPC Codes](https://doi.org/10.23919/date69613.2026.11539585)**
  Kai Zhang, Dingchao Gao, Zhaohui Yang, Runshi Zhou, Fangming Liu, Zhengfeng Ji, and Jianxin Chen. *Proceedings of the Design, Automation & Test in Europe Conference (DATE '26)*, 2026.
- **[Syndrome Extraction Circuits with Near-Optimal Depths for Practical Quantum Error Correcting Code Families](https://63dac.conference-program.com/presentation/?id=RESEARCH144&sess=sess134)**
  Daniel Bochen Tan, J. Pablo Bonilla Ataides, Varun Menon, Jin Ming Koh, Andrei C. Diaconu, and Mikhail D. Lukin. *Proceedings of the ACM/IEEE Design Automation Conference (DAC '26)*, 2026.
- **[High-performance syndrome extraction circuits for quantum codes](https://arxiv.org/abs/2603.05481)**
  Armands Strikis, Dan E Browne, and Michael E Beverland. *arXiv preprint arXiv:2603.05481*, 2026.
- **[Scheduling of syndrome measurements with a few ancillary qubits](https://arxiv.org/abs/2508.07913)**
  Shintaro Sato, and Yasunari Suzuki. *arXiv preprint arXiv:2508.07913*, 2025.

<a id="s2-2"></a>
### Hardware-Aware QEC Realization on Superconducting Circuits

- **[A Synthesis Framework for Stitching Surface Code with Superconducting Quantum Devices](https://doi.org/10.1145/3470496.3527381)**
  Anbang Wu, Gushu Li, Hezi Zhang, Gian Giacomo Guerreschi, Yufei Ding, and Yuan Xie. *Proceedings of the ACM/IEEE International Symposium on Computer Architecture (ISCA '22)*, 2022.
- **[QECC-synth: A Layout Synthesizer for Quantum Error Correction Codes on Sparse Hardware Architectures](https://arxiv.org/abs/2308.06428)**
  Keyi Yin, Hezi Zhang, Xiang Fang, Yunong Shi, Travis Humble, Ang Li, and Yufei Ding. *arXiv preprint arXiv:2308.06428*, 2024.
- **[Flag-proxy networks: Overcoming the architectural, scheduling and decoding obstacles of quantum ldpc codes](https://doi.org/10.1109/micro61859.2024.00059)**
  Suhas Vittal, Ali Javadi-Abhari, Andrew W Cross, Lev S Bishop, and Moinuddin Qureshi. *Proceedings of the IEEE/ACM International Symposium on Microarchitecture (MICRO '24)*, 2024.
- **[Louvre: Relaxing Hardware Requirements of Quantum LDPC Codes by Routing with Expanded Quantum Instruction Set](https://arxiv.org/abs/2508.20858)**
  Runshi Zhou, Fang Zhang, Hui-Hai Zhao, Feng Wu, Linghang Kong, and Jianxin Chen. *arXiv preprint arXiv:2508.20858*, 2025.
- **[A simple universal routing strategy for reducing the connectivity requirements of quantum LDPC codes](https://arxiv.org/abs/2509.00850)**
  Guangqi Zhao, Fei Yan, and Xiaotong Ni. *arXiv preprint arXiv:2509.00850*, 2025.
- **[Placing and routing quantum LDPC codes in multilayer superconducting hardware](https://doi.org/10.1038/s41534-026-01243-w)**
  Melvin Mathews, Lukas Pahl, David Pahl, Vaishnavi L Addala, Catherine Tang, William D Oliver, and Jeffrey A Grover. *npj Quantum Information*, 2026.
- **[CaliScalpel: In-Situ and Fine-Grained Qubit Calibration Integrated with Surface Code Quantum Error Correction](https://arxiv.org/abs/2412.02036)**
  Xiang Fang, Keyi Yin, Yuchen Zhu, Jixuan Ruan, Dean Tullsen, Zhiding Liang, Andrew Sornborger, Ang Li, et al. *arXiv preprint arXiv:2412.02036*, 2024.
- **[YOUTIAO: Hybrid Multiplexing with Dynamic Qubit Grouping for Low-cost and Scalable Quantum Wiring](https://doi.org/10.1145/3725843.3756061)**
  Wuwei Tian, Liqiang Lu, Siwei Tan, Shiyu Li, Hengyi Li, Tianyao Chu, Xuhong Zhang, Mingshuai Chen, et al. *Proceedings of the IEEE/ACM International Symposium on Microarchitecture (MICRO '25)*, 2025.
- **[Tiscc: A surface code compiler and resource estimator for trapped-ion processors](https://doi.org/10.1145/3624062.3624214)**
  Tyler LeBlond, Ryan S Bennink, Justin G Lietz, and Christopher M Seck. *Proceedings of the IEEE/ACM International Conference for High Performance Computing, Networking, Storage and Analysis Workshops (SC Workshops '23)*, 2023.
- **[Architecting scalable trapped ion quantum computers using surface codes](https://doi.org/10.1145/3779212.3790128)**
  Scott Jones, and Prakash Murali. *Proceedings of the ACM International Conference on Architectural Support for Programming Languages and Operating Systems (ASPLOS '26)*, 2026.
- **[iSwitch: QEC on Demand via In-Situ Encoding of Bare Qubits for Ion Trap Architectures](https://doi.org/10.1145/3779212.3790177)**
  Keyi Yin, Xiang Fang, Zhuo Chen, Ang Li, David Hayes, Eneet Kaur, Reza Nejabati, Hartmut Haeffner, et al. *Proceedings of the ACM International Conference on Architectural Support for Programming Languages and Operating Systems (ASPLOS '26)*, 2026.
- **[Ion-trap chip architecture optimized for the implementation of quantum error-correcting codes](https://doi.org/10.1103/tk99-76gb)**
  Jeonghoon Lee, Hyeongjun Jeon, and Taehyun Kim. *Physical Review A*, 2026.
- **[Moveless: Minimizing QEC Overhead on QCCDs via Versatile Execution and Low Excess Shuttling](https://doi.org/10.1109/qce65121.2025.00075)**
  Sahil Khan, Suhas Vittal, Kenneth Brown, and Jonathan Baker. *Proceedings of the IEEE International Conference on Quantum Computing and Engineering (QCE '25)*, 2025.
- **[Cyclone: Designing Efficient and Highly Parallel QCCD Architectural Codesigns for Fault Tolerant Quantum Memory](https://arxiv.org/abs/2511.15910)**
  Sahil Khan, Abhinav Anand, Kenneth R Brown, and Jonathan M Baker. *arXiv preprint arXiv:2511.15910*, 2025.
- **[Fault-Tolerant Quantum Computing with Trapped Ions: The Walking Cat Architecture](https://arxiv.org/abs/2604.19481)**
  Felix Tripier, Woo Chang Chung, Jacob Young, Safwan Alam, Bryce Bjork, Aharon Brodutch, Finn Lasse Buessen, Nolan J. Coble, et al. *arXiv preprint arXiv:2604.19481*, 2026.
- **[Logical quantum processor based on reconfigurable atom arrays](https://doi.org/10.1038/s41586-023-06927-3)**
  Dolev Bluvstein, Simon J Evered, Alexandra A Geim, Sophie H Li, Hengyun Zhou, Tom Manovitz, Sepehr Ebadi, Madelyn Cain, et al. *Nature*, 2024.
- **[An Architecture for Improved Surface Code Connectivity in Neutral Atoms](https://arxiv.org/abs/2309.13507)**
  Joshua Viszlai, Sophia Fuhui Lin, Siddharth Dangwal, Jonathan M. Baker, and Frederic T. Chong. *arXiv preprint arXiv:2309.13507*, 2023.
- **[qSIEVE: Efficient qLDPC Memory via Systolic Movement in Atom Arrays](https://doi.org/10.1145/3779066)**
  Joshua Viszlai, Willers Yang, Sophia Fuhui Lin, Junyu Liu, Natalia Nottingham, Jonathan M Baker, and Frederic T Chong. *ACM Transactions on Quantum Computing*, 2026.
- **[High-rate quantum LDPC codes for long-range-connected neutral atom registers](https://doi.org/10.1038/s41467-025-56255-5)**
  Laura Pecorari, Sven Jandura, Gavin K Brennen, and Guido Pupillo. *Nature Communications*, 2025.
- **[ConiQ: Enabling Concatenated Quantum Error Correction on Neutral Atom Arrays](https://doi.org/10.1109/qce65121.2025.00073)**
  Pengyu Liu, Mingkuan Xu, Hengyun Zhou, Hanrui Wang, Umut A. Acar, and Yunong Shi. *Proceedings of the IEEE International Conference on Quantum Computing and Engineering (QCE '25)*, 2025.
- **[An abstract model and efficient routing for logical entangling gates on zoned neutral atom architectures](https://doi.org/10.1109/qce60285.2024.00098)**
  Yannick Stade, Ludwig Schmid, Lukas Burgholzer, and Robert Wille. *Proceedings of the IEEE International Conference on Quantum Computing and Engineering (QCE '24)*, 2024.
- **[Quantum error correction resilient against atom loss](https://doi.org/10.22331/q-2025-10-13-1884)**
  Hugo Perrin, Sven Jandura, and Guido Pupillo. *Quantum*, 2025.
- **[Low-depth quantum error correction via three-qubit gates in Rydberg atom arrays](https://arxiv.org/abs/2507.06096)**
  Laura Pecorari, Sven Jandura, and Guido Pupillo. *arXiv preprint arXiv:2507.06096*, 2025.
- **[The IBM Quantum Heavy Hex Lattice](https://www.ibm.com/quantum/blog/heavy-hex-lattice)**
  IBM Quantum. 2021.
- **[Willow Spec Sheet](https://quantumai.google/static/site-assets/downloads/willow-spec-sheet.pdf)**
  Google Quantum AI. 2024.
- **[Amazon Braket Launches the Rigetti Ankaa-2 Superconducting Device](https://aws.amazon.com/blogs/quantum-computing/amazon-braket-launches-the-rigetti-ankaa-2-superconducting-device-2/)**
  Amazon Web Services. 2024.
- **[Fault-tolerant quantum error correction on near-term quantum processors using flag and bridge qubits](https://doi.org/10.1103/physreva.101.032333)**
  Lingling Lao, and Carmen G Almudever. *Physical Review A*, 2020.
- **[Relaxing hardware requirements for surface code circuits using time-dynamics](https://doi.org/10.22331/q-2023-11-07-1172)**
  Matt McEwen, Dave Bacon, and Craig Gidney. *Quantum*, 2023.
- **[Lowering connectivity requirements for bivariate bicycle codes using morphing circuits](https://doi.org/10.1103/physrevlett.134.090602)**
  Mackenzie H Shaw, and Barbara M Terhal. *Physical Review Letters*, 2025.
- **[Directional codes: a new family of quantum LDPC codes on hexagonal-and square-grid connectivity hardware](https://arxiv.org/abs/2507.19430)**
  Gyorgy P Geher, David Byfield, and Archibald Ruban. *arXiv preprint arXiv:2507.19430*, 2025.
- **[Constant-overhead quantum error correction with thin planar connectivity](https://doi.org/10.1103/physrevlett.129.050504)**
  Maxime A Tremblay, Nicolas Delfosse, and Michael E Beverland. *Physical Review Letters*, 2022.

<a id="s2-3"></a>
### Hardware-Aware QEC Realization on Trapped-Ion

- **[Architecting noisy intermediate-scale trapped ion quantum computers](https://doi.org/10.1109/isca45697.2020.00051)**
  Prakash Murali, Dripto M Debroy, Kenneth R Brown, and Margaret Martonosi. *Proceedings of the ACM/IEEE International Symposium on Computer Architecture (ISCA '20)*, 2020.
- **[Muzzle the shuttle: Efficient compilation for multi-trap trapped-ion quantum computers](https://doi.org/10.23919/date54114.2022.9774619)**
  Abdullah Ash Saki, Rasit Onur Topaloglu, and Swaroop Ghosh. *Proceedings of the Design, Automation & Test in Europe Conference (DATE '22)*, 2022.

<a id="s2-4"></a>
### Hardware-Aware QEC Realization on Neutral Atom Arrays

- **[Qubit Mapping for Reconfigurable Atom Arrays](https://doi.org/10.1145/3508352.3549331)**
  Bochen Tan, Dolev Bluvstein, Mikhail D Lukin, and Jason Cong. *Proceedings of the IEEE/ACM International Conference on Computer-Aided Design (ICCAD '22)*, 2022.
- **[Compiling Quantum Circuits for Dynamically Field-Programmable Neutral Atoms Array Processors](https://doi.org/10.22331/q-2024-03-14-1281)**
  Daniel Bochen Tan, Dolev Bluvstein, Mikhail D Lukin, and Jason Cong. *Quantum*, 2024.
- **[Compilation for Dynamically Field-Programmable Qubit Arrays with Efficient and Provably Near-Optimal Scheduling](https://arxiv.org/abs/2405.15095)**
  Daniel Bochen Tan, Wan-Hsuan Lin, and Jason Cong. *arXiv preprint arXiv:2405.15095*, 2024.
- **[Q-Pilot: field programmable quantum array compilation with flying ancillas](https://arxiv.org/abs/2311.16190)**
  Hanrui Wang, Bochen Tan, Pengyu Liu, Yilian Liu, Jiaqi Gu, Jason Cong, and Song Han. *arXiv preprint arXiv:2311.16190*, 2023.
- **[Atomique: A quantum compiler for reconfigurable neutral atom arrays](https://doi.org/10.1109/isca59077.2024.00030)**
  Hanrui Wang, Pengyu Liu, Daniel Bochen Tan, Yilian Liu, Jiaqi Gu, David Z Pan, Jason Cong, Umut A Acar, et al. *Proceedings of the ACM/IEEE International Symposium on Computer Architecture (ISCA '24)*, 2024.
- **[Circuit decompositions and scheduling for neutral atom devices with limited local addressability](https://doi.org/10.1109/qce60285.2024.00105)**
  Natalia Nottingham, Michael A Perlin, Dhirpal Shah, Ryan White, Hannes Bernien, Frederic T Chong, and Jonathan M Baker. *Proceedings of the IEEE International Conference on Quantum Computing and Engineering (QCE '24)*, 2024.
- **[Reuse-aware compilation for zoned quantum architectures based on neutral atoms](https://arxiv.org/abs/2411.11784)**
  Wan-Hsuan Lin, Daniel Bochen Tan, and Jason Cong. *arXiv preprint arXiv:2411.11784*, 2024.
- **[PowerMove: Optimizing Compilation for Neutral Atom Quantum Computers with Zoned Architecture](https://doi.org/10.1145/3676642.3736128)**
  Jixuan Ruan, Xiang Fang, Hezi Zhang, Ang Li, Travis Humble, and Yufei Ding. *Proceedings of the ACM International Conference on Architectural Support for Programming Languages and Operating Systems (ASPLOS '25)*, 2025.
- **[ZAP: Zoned Architecture and Performant Compiler for Field Programmable Atom Array](https://doi.org/10.1109/tqe.2026.3696707)**
  Chen Huang, Xi Zhao, Hongze Xu, Weifeng Zhuang, Meng-Jun Hu, Dong E Liu, and Jingbo Wang. *IEEE Transactions on Quantum Engineering*, 2026.
- **[A quantum processor based on coherent transport of entangled atom arrays](https://doi.org/10.1038/s41586-022-04592-6)**
  Dolev Bluvstein, Harry Levine, Giulia Semeghini, Tout T. Wang, Sepehr Ebadi, Marcin Kalinowski, Alexander Keesling, Nishad Maskara, et al. *Nature*, 2022.
- **[Fault-tolerant quantum computation with a neutral atom processor](https://arxiv.org/abs/2411.11822)**
  Ben W Reichardt, Adam Paetznick, David Aasen, Ivan Basov, Juan M Bello-Rivas, Parsa Bonderson, Rui Chao, Wim van Dam, et al. *arXiv preprint arXiv:2411.11822*, 2024.
- **[A fault-tolerant neutral-atom architecture for universal quantum computation](https://doi.org/10.1038/s41586-025-09848-5)**
  Dolev Bluvstein, Alexandra A. Geim, Sophie H. Li, Simon J. Evered, J. Pablo Bonilla Ataides, Gefen Baranes, Andi Gu, Tom Manovitz, et al. *Nature*, 2026.
- **[Circuit-based leakage-to-erasure conversion in a neutral-atom quantum processor](https://doi.org/10.1103/prxquantum.5.040343)**
  Matthew NH Chow, Vikas Buchemmavari, Sivaprasad Omanakuttan, Bethany J Little, Saurabh Pandey, Ivan H Deutsch, and Yuan-Yu Jau. *PRX Quantum*, 2024.
- **[Long-range-enhanced surface codes](https://doi.org/10.1103/physreva.110.022607)**
  Yifan Hong, Matteo Marinelli, Adam M Kaufman, and Andrew Lucas. *Physical Review A*, 2024.
- **[Transversal Logical Clifford gates on rotated surface codes with reconfigurable neutral atom arrays](https://arxiv.org/abs/2412.01391)**
  Zi-Han Chen, Ming-Cheng Chen, Chao-Yang Lu, and Jian-Wei Pan. *arXiv preprint arXiv:2412.01391*, 2024.
- **[Constant-Overhead Fault-Tolerant Quantum Computation with Reconfigurable Atom Arrays](https://doi.org/10.1038/s41567-024-02479-z)**
  Qian Xu, J. Pablo Bonilla Ataides, Christopher A. Pattison, Nithin Raveendran, Dolev Bluvstein, Jonathan Wurtz, Bane Vasic, Mikhail D. Lukin, et al. *Nature Physics*, 2024.
- **[Towards Ultra-High-Rate Quantum Error Correction with Reconfigurable Atom Arrays](https://arxiv.org/abs/2604.16209)**
  Chen Zhao, Casey Duckering, Andi Gu, Nishad Maskara, and Hengyun Zhou. *arXiv preprint arXiv:2604.16209*, 2026.
- **[High-threshold codes for neutral-atom qubits with biased erasure errors](https://doi.org/10.1103/physrevx.13.041013)**
  Kaavya Sahay, Junlan Jin, Jahan Claes, Jeff D. Thompson, and Shruti Puri. *Physical Review X*, 2023.
- **[Quantum low-density parity-check codes for erasure-biased atomic quantum processors](https://doi.org/10.1103/mgkt-ctv8)**
  Laura Pecorari, and Guido Pupillo. *Physical Review A*, 2025.
- **[Optimal state preparation for logical arrays on zoned neutral atom quantum computers](https://doi.org/10.23919/date64628.2025.10993241)**
  Yannick Stade, Ludwig Schmid, Lukas Burgholzer, and Robert Wille. *Proceedings of the Design, Automation & Test in Europe Conference (DATE '25)*, 2025.
- **[Efficient fault-tolerant implementations of non-Clifford gates with reconfigurable atom arrays](https://doi.org/10.1038/s41534-024-00945-3)**
  Yifei Wang, Yixu Wang, Yu-An Chen, Wenjun Zhang, Tao Zhang, Jiazhong Hu, Wenlan Chen, Yingfei Gu, et al. *npj Quantum Information*, 2024.
- **[Erasure conversion for fault-tolerant quantum computing in alkaline earth Rydberg atom arrays](https://doi.org/10.1038/s41467-022-32094-6)**
  Yue Wu, Shimon Kolkowitz, Shruti Puri, and Jeff D. Thompson. *Nature Communications*, 2022.
- **[Erasure-tolerance scheme for the surface codes on neutral atom quantum computers](https://doi.org/10.1109/tqe.2025.3627918)**
  Fumiyoshi Kobayashi, and Shota Nagayama. *IEEE Transactions on Quantum Engineering*, 2025.
- **[Correlated Atom Loss as a Resource for Quantum Error Correction](https://arxiv.org/abs/2603.24237)**
  Hugo Perrin, Gatien Roger, and Guido Pupillo. *arXiv preprint arXiv:2603.24237*, 2026.
- **[Resource Analysis of Low-Overhead Transversal Architectures for Reconfigurable Atom Arrays](https://doi.org/10.1145/3695053.3731039)**
  Hengyun Zhou, Casey Duckering, Chen Zhao, Dolev Bluvstein, Madelyn Cain, Aleksander Kubica, Sheng-Tao Wang, and Mikhail D. Lukin. *Proceedings of the ACM/IEEE International Symposium on Computer Architecture (ISCA '25)*, 2025.

<a id="s3"></a>
## Decoding in the Classical Control Stack

<a id="s3-1"></a>
### Algorithms and Implementation Trade-offs

<a id="s3-1-1"></a>
#### Algorithm Overview and Tools

- **[Hardness of Decoding Quantum Stabilizer Codes](https://doi.org/10.1109/tit.2015.2422294)**
  Pavithran Iyer, and David Poulin. *IEEE Transactions on Information Theory*, 2015.
- **[Decoding Algorithms for Surface Codes](https://doi.org/10.22331/q-2024-10-10-1498)**
  Antonio deMarti iOlius, Patricio Fuentes, Roman Orus, Pedro M. Crespo, and Josu Etxezarreta Martinez. *Quantum*, 2024.
- **[Efficient Algorithms for Maximum Likelihood Decoding in the Surface Code](https://doi.org/10.1103/physreva.90.032326)**
  Sergey Bravyi, Martin Suchara, and Alexander Vargo. *Physical Review A*, 2014.
- **[Statistical Mechanical Models for Quantum Codes with Correlated Noise](https://doi.org/10.4171/aihpd/105)**
  Christopher T. Chubb, and Steven T. Flammia. *Annales de l'Institut Henri Poincare D*, 2021.
- **[Stim: a fast stabilizer circuit simulator](https://doi.org/10.22331/q-2021-07-06-497)**
  Craig Gidney. *Quantum*, 2021.
- **[LightStim: A Framework for QEC Protocol Evaluation and Prototyping with Automated DEM Construction](https://arxiv.org/abs/2604.21472)**
  Xiang Fang, Ming Wang, Yue Wu, Sharanya Prabhu, Dean Tullsen, Narasinga Rao Miniskar, Frank Mueller, Travis Humble, et al. 2026.

<a id="s3-1-2"></a>
#### Matching / MWPM

- **[Paths, Trees, and Flowers](https://doi.org/10.4153/cjm-1965-045-4)**
  Jack Edmonds. *Canadian Journal of Mathematics*, 1965.
- **[Sparse blossom: correcting a million errors per core second with minimum-weight matching](https://doi.org/10.22331/q-2025-01-20-1600)**
  Oscar Higgott, and Craig Gidney. *Quantum*, 2025.
- **[Fusion Blossom: Fast MWPM Decoders for QEC](https://doi.org/10.1109/qce57702.2023.00107)**
  Yue Wu, and Lin Zhong. *Proceedings of the IEEE International Conference on Quantum Computing and Engineering (QCE '23)*, 2023.
- **[Improved decoding of circuit noise and fragile boundaries of tailored surface codes](https://doi.org/10.1103/physrevx.13.031007)**
  Oscar Higgott, Thomas C Bohdanowicz, Aleksander Kubica, Steven T Flammia, and Earl T Campbell. *Physical Review X*, 2023.
- **[Belief propagation as a partial decoder](https://arxiv.org/abs/2306.17142)**
  Laura Caune, Brendan Reid, Joan Camps, and Earl Campbell. *arXiv preprint arXiv:2306.17142*, 2023.
- **[Multi-path summation for decoding 2D topological codes](https://doi.org/10.22331/q-2018-10-19-102)**
  Ben Criger, and Imran Ashraf. *Quantum*, 2018.
- **[Optimal complexity correction of correlated errors in the surface code](https://arxiv.org/abs/1310.0863)**
  Austin G Fowler. *arXiv preprint arXiv:1310.0863*, 2013.
- **[Pipelined correlated minimum weight perfect matching of the surface code](https://doi.org/10.22331/q-2023-12-12-1205)**
  Alexandru Paler, and Austin G Fowler. *Quantum*, 2023.
- **[Enhancing Fault-Tolerant Surface Code Decoding with Iterative Lattice Reweighting](https://arxiv.org/abs/2509.06756)**
  Yi Tian, Y Zheng, Xiaoting Wang, and Ching-Yi Lai. *arXiv preprint arXiv:2509.06756*, 2025.
- **[Micro Blossom: Accelerated Minimum-Weight Perfect Matching Decoding for Quantum Error Correction](https://doi.org/10.1145/3676641.3716005)**
  Yue Wu, Namitha Liyanage, and Lin Zhong. *Proceedings of the ACM International Conference on Architectural Support for Programming Languages and Operating Systems (ASPLOS '25)*, 2025.
- **[Astrea: Accurate Quantum Error-Decoding via Practical Minimum-Weight Perfect-Matching](https://doi.org/10.1145/3579371.3589037)**
  Suhas Vittal, Poulami Das, and Moinuddin Qureshi. *Proceedings of the ACM/IEEE International Symposium on Computer Architecture (ISCA '23)*, 2023.
- **[LILLIPUT: A Lightweight Low-Latency Lookup-Table Decoder for Near-Term Quantum Error Correction](https://doi.org/10.1145/3503222.3507707)**
  Poulami Das, Aditya Locharla, and Cody Jones. *Proceedings of the ACM International Conference on Architectural Support for Programming Languages and Operating Systems (ASPLOS '22)*, 2022.
- **[NISQ+: Boosting Quantum Computing Power by Approximating Quantum Error Correction](https://doi.org/10.1109/isca45697.2020.00053)**
  Adam Holmes, Mohammad Reza Jokar, Ghasem Pasandi, Yongshan Ding, Massoud Pedram, and Frederic T. Chong. *Proceedings of the ACM/IEEE International Symposium on Computer Architecture (ISCA '20)*, 2020.
- **[QECOOL: On-Line Quantum Error Correction with a Superconducting Decoder for Surface Code](https://doi.org/10.1109/dac18074.2021.9586326)**
  Yosuke Ueno, Masaaki Kondo, Masamitsu Tanaka, Yasunari Suzuki, and Yutaka Tabuchi. *Proceedings of the ACM/IEEE Design Automation Conference (DAC '21)*, 2021.
- **[QULATIS: A Quantum Error Correction Methodology toward Lattice Surgery](https://doi.org/10.1109/hpca53966.2022.00028)**
  Yosuke Ueno, Masaaki Kondo, Masamitsu Tanaka, Yasunari Suzuki, and Yutaka Tabuchi. *Proceedings of the IEEE International Symposium on High-Performance Computer Architecture (HPCA '22)*, 2022.
- **[Better Than Worst-Case Decoding for Quantum Error Correction](https://doi.org/10.1145/3575693.3575733)**
  Gokul Subramanian Ravi, Jonathan M. Baker, Arash Fayyazi, Sophia Fuhui Lin, Ali Javadi-Abhari, Massoud Pedram, and Frederic T. Chong. *Proceedings of the ACM International Conference on Architectural Support for Programming Languages and Operating Systems (ASPLOS '23)*, 2023.
- **[Promatch: Extending the Reach of Real-Time Quantum Error Correction with Adaptive Predecoding](https://doi.org/10.1145/3620666.3651339)**
  Narges Alavisamani, Suhas Vittal, Ramin Ayanzadeh, Poulami Das, and Moinuddin Qureshi. *Proceedings of the ACM International Conference on Architectural Support for Programming Languages and Operating Systems (ASPLOS '24)*, 2024.
- **[Pinball: A Cryogenic Predecoder for Quantum Error Correction Decoding Under Circuit-Level Noise](https://doi.org/10.1109/hpca68181.2026.11408464)**
  Alexander Knapen, Guanchen Tao, Jacob Mack, Tomas Bruno, Mehdi Saligane, Dennis Sylvester, Qirui Zhang, and Gokul Subramanian Ravi. *Proceedings of the IEEE International Symposium on High-Performance Computer Architecture (HPCA '26)*, 2026.
- **[PyMatching: A Python package for decoding quantum codes with minimum-weight perfect matching](https://doi.org/10.1145/3505637)**
  Oscar Higgott. *ACM Transactions on Quantum Computing*, 2022.
- **[Minimising surface-code failures using a color-code decoder](https://doi.org/10.22331/q-2025-02-17-1632)**
  Asmae Benhemou, Kaavya Sahay, Lingling Lao, and Benjamin J. Brown. *Quantum*, 2025.
- **[The correlated matching decoder for the 4.8.8 color code](https://doi.org/10.1007/s11432-026-5027-y)**
  Yantong Liu, Junjie Wu, and Lingling Lao. *Science China Information Sciences*, 2026.

<a id="s3-1-3"></a>
#### Search and Hypergraph Decoding

- **[Tesseract: A Search-Based Decoder for Quantum Error Correction](https://arxiv.org/abs/2503.10988)**
  Laleh Aghababaie Beni, Oscar Higgott, and Noah Shutty. *arXiv preprint arXiv:2503.10988*, 2025.
- **[Minimum-Weight Parity Factor Decoder for Quantum Error Correction](https://arxiv.org/abs/2508.04969)**
  Yue Wu, Binghong Li, Kathleen Chang, Shruti Puri, and Lin Zhong. *arXiv preprint arXiv:2508.04969*, 2025.

<a id="s3-1-4"></a>
#### Union-Find

- **[Almost-linear time decoding algorithm for topological codes](https://doi.org/10.22331/q-2021-12-02-595)**
  Nicolas Delfosse, and Naomi H Nickerson. *Quantum*, 2021.
- **[Fault-tolerant weighted union-find decoding on the toric code](https://doi.org/10.1103/physreva.102.012419)**
  Shilin Huang, Michael Newman, and Kenneth R Brown. *Physical Review A*, 2020.
- **[Toward a union-find decoder for quantum LDPC codes](https://doi.org/10.1109/tit.2022.3143452)**
  Nicolas Delfosse, Vivien Londe, and Michael E Beverland. *IEEE Transactions on Information Theory*, 2022.
- **[Actis: a strictly local union-find decoder](https://doi.org/10.22331/q-2023-11-14-1183)**
  Tim Chan, and Simon C Benjamin. *Quantum*, 2023.
- **[AFS: Accurate, Fast, and Scalable Error-Decoding for Fault-Tolerant Quantum Computers](https://doi.org/10.1109/hpca53966.2022.00027)**
  Poulami Das, Christopher A. Pattison, Srilatha Manne, Douglas M. Carmean, Krysta M. Svore, Moinuddin Qureshi, and Nicolas Delfosse. *Proceedings of the IEEE International Symposium on High-Performance Computer Architecture (HPCA '22)*, 2022.
- **[Scalable quantum error correction for surface codes using FPGA](https://doi.org/10.1109/qce57702.2023.00106)**
  Namitha Liyanage, Yue Wu, Alexander Deters, and Lin Zhong. *Proceedings of the IEEE International Conference on Quantum Computing and Engineering (QCE '23)*, 2023.
- **[A real-time, scalable, fast and resource-efficient decoder for a quantum computer](https://doi.org/10.1038/s41928-024-01319-5)**
  Ben Barber, Kenton M. Barnes, Tomasz Bialas, Okan Bugdayc, Earl T. Campbell, Neil I. Gillespie, Kauser Johar, Ram Rajan, et al. *Nature Electronics*, 2025.
- **[Network-Integrated Decoding System for Real-Time Quantum Error Correction with Lattice Surgery](https://doi.org/10.1109/qce65121.2025.00129)**
  Namitha Liyanage, Yue Wu, Emmet Houghton, and Lin Zhong. *Proceedings of the IEEE International Conference on Quantum Computing and Engineering (QCE '25)*, 2025.
- **[Coset Ensemble Decoder for Quantum Error Correction with Algorithm-Hardware Co-Design](https://arxiv.org/abs/2606.11076)**
  Shuang Liang, Jubo Xu, Giulio Bassanino, Qianzhou Wang, Yidong Zhou, Yuncheng Lu, Zhiwen Mo, Paul H. J. Kelly, et al. *Proceedings of the ACM/IEEE International Symposium on Computer Architecture (ISCA '26)*, 2026.
- **[Local clustering decoder as a fast and adaptive hardware decoder for the surface code](https://doi.org/10.1038/s41467-025-66773-x)**
  Abbas B. Ziad, Ankit Zalawadiya, Canberk Topal, Joan Camps, Gyorgy P. Geher, Matthew P. Stafford, and Mark L. Turner. *Nature Communications*, 2025.

<a id="s3-1-5"></a>
#### Belief Propagation

- **[Exploiting degeneracy in belief propagation decoding of quantum codes](https://doi.org/10.1038/s41534-022-00623-2)**
  Kao-Yueh Kuo, and Ching-Yi Lai. *npj Quantum Information*, 2022.
- **[On the iterative decoding of sparse quantum codes](https://arxiv.org/abs/0801.1241)**
  David Poulin, and Yeojin Chung. *arXiv preprint arXiv:0801.1241*, 2008.
- **[Decoding Across the Quantum Low-Density Parity-Check Code Landscape](https://doi.org/10.1103/physrevresearch.2.043423)**
  Joschka Roffe, David R. White, Simon Burton, and Earl T. Campbell. *Physical Review Research*, 2020.
- **[Degenerate quantum LDPC codes with good finite length performance](https://doi.org/10.22331/q-2021-11-22-585)**
  Pavel Panteleev, and Gleb Kalachev. *Quantum*, 2021.
- **[Localized statistics decoding for quantum low-density parity-check codes](https://doi.org/10.1038/s41467-025-63214-7)**
  Timo Hillmann, Lucas Berent, Armanda O. Quintavalle, Jens Eisert, Robert Wille, and Joschka Roffe. *Nature Communications*, 2025.
- **[An almost-linear time decoding algorithm for quantum LDPC codes under circuit-level noise](https://arxiv.org/abs/2409.01440)**
  Antonio deMarti iOlius, Imanol Etxezarreta Martinez, Joschka Roffe, and Josu Etxezarreta Martinez. *arXiv preprint arXiv:2409.01440*, 2024.
- **[Introducing Ambiguity Clustering: An Accurate and Efficient Decoder for qLDPC Codes](https://doi.org/10.1109/qce60285.2024.10326)**
  Stasiu Wolanski, and Ben Barber. *Proceedings of the IEEE International Conference on Quantum Computing and Engineering (QCE '24)*, 2024.
- **[Belief propagation decoding of quantum LDPC codes with guided decimation](https://doi.org/10.1109/isit57864.2024.10619083)**
  Hanwen Yao, Waleed Abu Laban, Christian Hager, Alexandre Graell i Amat, and Henry D Pfister. *Proceedings of the IEEE International Symposium on Information Theory (ISIT '24)*, 2024.
- **[Beam search decoder for quantum LDPC codes](https://arxiv.org/abs/2512.07057)**
  Min Ye, Dave Wecker, and Nicolas Delfosse. *arXiv preprint arXiv:2512.07057*, 2025.
- **[Improved belief propagation is sufficient for real-time decoding of quantum memory](https://arxiv.org/abs/2506.01779)**
  Tristan Muller, Thomas Alexander, Michael E Beverland, Markus Buhler, Blake R Johnson, Thilo Maurer, and Drew Vandeth. *arXiv preprint arXiv:2506.01779*, 2025.
- **[Improved belief propagation decoding algorithms for surface codes](https://doi.org/10.1109/tqe.2025.3577769)**
  Jiahan Chen, Zhengzhong Yi, Zhipeng Liang, and Xuan Wang. *IEEE Transactions on Quantum Engineering*, 2025.
- **[Restart Belief: A General Quantum LDPC Decoder](https://doi.org/10.1109/lcomm.2026.3666352)**
  Lorenzo Valentini, Diego Forlivesi, Andrea Talarico, and Marco Chiani. *IEEE Communications Letters*, 2026.
- **[Fully Parallelized BP Decoding for Quantum LDPC Codes Can Outperform BP-OSD](https://doi.org/10.1109/hpca68181.2026.11408621)**
  Ming Wang, Ang Li, and Frank Mueller. *Proceedings of the IEEE International Symposium on High-Performance Computer Architecture (HPCA '26)*, 2026.
- **[Automorphism Ensemble Decoding of Quantum LDPC Codes](https://arxiv.org/abs/2503.01738)**
  Stergios Koutsioumpas, Hasan Sayginel, Mark Webster, and Dan E. Browne. *arXiv preprint arXiv:2503.01738*, 2025.
- **[Decoding correlated errors in quantum LDPC codes](https://doi.org/10.1038/s41467-026-70556-3)**
  Arshpreet Singh Maan, Francisco Miguel Garcia Herrero, Alexandru Paler, and Valentin Savin. *Nature Communications*, 2026.
- **[Colour Codes Reach Surface Code Performance using Vibe Decoding](https://arxiv.org/abs/2508.15743)**
  Stergios Koutsioumpas, Tamas Noszko, Hasan Sayginel, Mark Webster, and Joschka Roffe. *arXiv preprint arXiv:2508.15743*, 2025.
- **[Real-time decoding of the gross code memory with FPGAs](https://arxiv.org/abs/2510.21600)**
  Thilo Maurer, Markus Buhler, Michael Kroner, Frank Haverkamp, Tristan Muller, Drew Vandeth, and Blake R Johnson. *arXiv preprint arXiv:2510.21600*, 2025.
- **[Vegapunk: Accurate and Fast Decoding for Quantum LDPC Codes with Online Hierarchical Algorithm and Sparse Accelerator](https://doi.org/10.1145/3725843.3756084)**
  Kaiwen Zhou, Liqiang Lu, Debin Xiang, Chenning Tao, Anbang Wu, Jingwen Leng, Fangxin Liu, Mingshuai Chen, et al. *Proceedings of the IEEE/ACM International Symposium on Microarchitecture (MICRO '25)*, 2025.
- **[Syndrome-based min-sum vs OSD-0 decoders: FPGA implementation and analysis for quantum LDPC codes](https://doi.org/10.1109/access.2021.3118544)**
  Javier Valls, Francisco Garcia-Herrero, Nithin Raveendran, and Bane Vasic. *IEEE Access*, 2021.
- **[A Scalable FPGA Architecture for Real-Time Decoding of Quantum LDPC Codes Using GARI](https://arxiv.org/abs/2605.01035)**
  Daniel Bascones, Arshpreet Singh Maan, Valentin Savin, and Francisco Garcia-Herrero. *arXiv preprint arXiv:2605.01035*, 2026.

<a id="s3-1-6"></a>
#### Neural-Network Decoding

- **[Learning high-accuracy error decoding for quantum processors](https://doi.org/10.1038/s41586-024-08148-8)**
  Johannes Bausch, Andrew W Senior, Francisco JH Heras, Thomas Edlich, Alex Davies, Michael Newman, Cody Jones, Kevin Satzinger, et al. *Nature*, 2024.
- **[Efficient and universal neural-network decoder for stabilizer-based quantum error correction](https://arxiv.org/abs/2502.19971)**
  Gengyuan Hu, Wanli Ouyang, Chao-Yang Lu, Chen Lin, and Han-Sen Zhong. *arXiv preprint arXiv:2502.19971*, 2025.
- **[Scalable Neural Decoders for Practical Real-Time Quantum Error Correction](https://arxiv.org/abs/2510.22724)**
  Changwon Lee, Tak Hur, and Daniel K. Park. *arXiv preprint arXiv:2510.22724*, 2025.
- **[Sparse Mamba Decoder for Quantum Error Correction: Efficient Defect-Centric Processing of Surface Code Syndromes](https://arxiv.org/abs/2605.17156)**
  Samira Sayedsalehi, Nader Bagherzadeh, Maxim Shcherbakov, and Jean-Luc Gaudiot. *arXiv preprint arXiv:2605.17156*, 2026.
- **[SAQ: Stabilizer-Aware Quantum Error Correction Decoder](https://arxiv.org/abs/2512.08914)**
  David Zenati, and Eliya Nachmani. *arXiv preprint arXiv:2512.08914*, 2025.
- **[DiffQEC: A versatile diffusion model for quantum error correction](https://arxiv.org/abs/2604.24640)**
  Tianyi Xu, Qinglong Liu, Maolin Wang, Fei Zhang, Zhe Zhao, Yang Wang, and Ye Wei. *arXiv preprint arXiv:2604.24640*, 2026.
- **[Fast and accurate AI-based pre-decoders for surface codes](https://arxiv.org/abs/2604.12841)**
  Christopher Chamberland, Jan Olle, Muyuan Li, Scott Thornton, and Igor Baratta. *arXiv preprint arXiv:2604.12841*, 2026.
- **[Real-time Surface-Code Error Correction Using an FPGA-based Neural-Network Decoder](https://arxiv.org/abs/2605.04892)**
  Xiaohan Yang, Xuandong Sun, Zhiyi Wu, Jiawei Zhang, Ji Jiang, Xiayu Linpeng, Yuxuan Zhou, Ji Chu, et al. *arXiv preprint arXiv:2605.04892*, 2026.
- **[Rethink the Role of Neural Decoders in Quantum Error Correction](https://arxiv.org/abs/2605.12046)**
  Ge Yan, Shanchuan Li, and Yuxuan Du. *arXiv preprint arXiv:2605.12046*, 2026.
- **[A scalable and real-time neural decoder for topological quantum codes](https://doi.org/10.48550/arxiv.2512.07737)**
  Andrew W. Senior, Thomas Edlich, Francisco J. H. Heras, Lei M. Zhang, Oscar Higgott, James S. Spencer, et al. *arXiv preprint arXiv:2512.07737*, 2025.
- **[Learning to Decode in Parallel: Self-Coordinating Neural Network for Real-Time Quantum Error Correction](https://doi.org/10.48550/arxiv.2601.09921)**
  Kai Zhang, Zhengzhong Yi, Shaojun Guo, Linghang Kong, Situ Wang, Xiaoyu Zhan, Tan He, Weiping Lin, et al. *arXiv preprint arXiv:2601.09921*, 2026.
- **[Neural Decoders for Universal Quantum Algorithms](https://doi.org/10.1103/vjn1-mbxl)**
  J. Pablo Bonilla Ataides, Andi Gu, Susanne F. Yelin, and Mikhail D. Lukin. *PRX Intelligence*, 2026.
- **[Fast and Accurate AI-Based Pre-Decoders for Color Codes](https://arxiv.org/abs/2607.10058)**
  Jan Olle, Christopher Chamberland, Muyuan Li, and Igor Baratta. 2026.
- **[Ising-Decoding: A Set of Training Recipes for AI Quantum Error Correction Decoders](https://github.com/NVIDIA/Ising-Decoding)**
  NVIDIA Corporation. 2026.

<a id="s3-2"></a>
### Real-Time Decoder Integration

<a id="s3-2-1"></a>
#### Runtime Integration Overview

- **[Real-Time Decoding for Fault-Tolerant Quantum Computing: Progress, Challenges and Outlook](https://doi.org/10.1088/2399-1984/aceba6)**
  Francesco Battistel, Christopher Chamberland, Kauser Johar, Ramon W. J. Overwater, Fabio Sebastiano, Luka Skoric, Yosuke Ueno, and Muhammad Usman. *Nano Futures*, 2023.
- **[Demonstrating Real-Time and Low-Latency Quantum Error Correction with Superconducting Qubits](https://doi.org/10.1038/s41467-026-73331-6)**
  Laura Caune, Luka Skoric, Nick S. Blunt, Archibald Ruban, Jimmy McDaniel, Joseph A. Valery, Andrew D. Patterson, Alexander V. Gramolin, et al. *Nature Communications*, 2026.
- **[deq: Dynamic and Generic QEC Decoding System](https://github.com/microsoft/qdk-ec/tree/main/deq)**
  Microsoft. 2026.
- **[CUDA-QX](https://github.com/NVIDIA/cudaqx)**
  NVIDIA Corporation, CUDA-QX Development Team. 2026.
- **[CUDA-Q](https://github.com/NVIDIA/cuda-quantum)**
  The CUDA-Q Development Team. 2026.
- **[Platform Architecture for Tight Coupling of High-Performance Computing with Quantum Processors](https://arxiv.org/abs/2510.25213)**
  Shane A. Caldwell, Moein Khazraee, Elena Agostini, Tom Lassiter, Corey Simpson, Omri Kahalon, Mrudula Kanuri, Jin-Sung Kim, et al. 2025.
- **[Introducing cudaq-realtime for Programming the Logical QPU](https://nvidia.github.io/cuda-quantum/blogs/blog/2026/03/16/launching-cudaq-realtime/)**
  Shane Caldwell, Chuck Ketcham, Thien Nguyen, Bruno Schmitt, Elena Agostini, Corey Simpson, Jeffrey Bonde, Tom Lassiter, et al. 2026.
- **[Real-time decoding of quantum error correction codes using high-performance computing](https://doi.org/10.48550/arxiv.2608.03948)**
  Lingling Lao, Qiang Wang, Yuanqi Liu, Yantong Liu, Haowen Wang, Yitao Chen, Yankang Zhao, Zhenwei Wu, et al. *arXiv preprint arXiv:2608.03948*, 2026.

<a id="s3-2-2"></a>
#### Pauli and Clifford Frames

- **[Quantum computing with realistically noisy devices](https://doi.org/10.1038/nature03350)**
  Emanuel Knill. *Nature*, 2005.
- **[Effective fault-tolerant quantum computation with slow measurements](https://doi.org/10.1103/physrevlett.98.020501)**
  David P DiVincenzo, and Panos Aliferis. *Physical review letters*, 2007.
- **[Pauli frames for quantum computer architectures](https://doi.org/10.1145/3061639.3062300)**
  Leon Riesebos, Xiang Fu, Savvas Varsamopoulos, Carmen G Almudever, and Koen Bertels. *Proceedings of the ACM/IEEE Design Automation Conference (DAC '17)*, 2017.
- **[Quantum Teleportation Is a Universal Computational Primitive](https://arxiv.org/abs/quant-ph/9908010)**
  Daniel Gottesman, and Isaac L Chuang. *arXiv preprint quant-ph/9908010*, 1999.
- **[Flexible layout of surface code computations using AutoCCZ states](https://arxiv.org/abs/1905.08916)**
  Craig Gidney, and Austin G. Fowler. *arXiv preprint arXiv:1905.08916*, 2019.
- **[Quantum error correction below the surface code threshold](https://doi.org/10.1038/s41586-024-08449-y)**
  Rajeev Acharya, Dmitry A Abanin, Laleh Aghababaie-Beni, Igor Aleiner, Trond I Andersen, Markus Ansmann, Frank Arute, Kunal Arya, et al. *Nature*, 2025.
- **[Fault-tolerant quantum computing in the Pauli or Clifford frame with slow error diagnostics](https://doi.org/10.22331/q-2018-01-04-43)**
  Christopher Chamberland, Pavithran Iyer, and David Poulin. *Quantum*, 2018.
- **[Hard decoding algorithm for optimizing thresholds under general markovian noise](https://doi.org/10.1103/physreva.95.042332)**
  Christopher Chamberland, Joel Wallman, Stefanie Beale, and Raymond Laflamme. *Physical Review A*, 2017.

<a id="s3-2-3"></a>
#### Window and Speculative Decoding

- **[Modular decoding: parallelizable real-time decoding for quantum computers](https://arxiv.org/abs/2303.04846)**
  Hector Bombin, Chris Dawson, Ye-Hua Liu, Naomi Nickerson, Fernando Pastawski, and Sam Roberts. *arXiv preprint arXiv:2303.04846*, 2023.
- **[Spatially parallel decoding for multi-qubit lattice surgery](https://doi.org/10.1088/2058-9565/adc6b6)**
  Sophia Fuhui Lin, Eric C. Peterson, Krishanu Sankar, and Prasahnt Sivarajah. *Quantum Science and Technology*, 2025.
- **[ADaPT: Adaptive-window Decoding for Practical fault-Tolerance](https://arxiv.org/abs/2605.01149)**
  Tina Oberoi, Joshua Viszlai, and Frederic T. Chong. *arXiv preprint arXiv:2605.01149*, 2026.
- **[Adaptive Window Decoding based on Spatiotemporal Complementary Gap](https://arxiv.org/abs/2605.14637)**
  Moeto Mishima, Riki Toshio, Kaito Kishi, Jun Fujisaki, Hirotaka Oshima, Shintaro Sato, and Keisuke Fujii. *arXiv preprint arXiv:2605.14637*, 2026.
- **[Topological quantum memory](https://doi.org/10.1063/1.1499754)**
  Eric Dennis, Alexei Kitaev, Andrew Landahl, and John Preskill. *Journal of Mathematical Physics*, 2002.
- **[Parallel window decoding enables scalable fault tolerant quantum computation](https://doi.org/10.1038/s41467-023-42482-1)**
  Luka Skoric, Dan E Browne, Kenton M Barnes, Neil I Gillespie, and Earl T Campbell. *Nature Communications*, 2023.
- **[Scalable surface-code decoders with parallelization in time](https://doi.org/10.1103/prxquantum.4.040344)**
  Xinyu Tan, Fang Zhang, Rui Chao, Yaoyun Shi, and Jianxin Chen. *PRX Quantum*, 2023.
- **[SWIPER: Minimizing Fault-Tolerant Quantum Program Latency via Speculative Window Decoding](https://doi.org/10.1145/3695053.3731022)**
  Joshua Viszlai, Jason D Chadwick, Sarang Joshi, Gokul Subramanian Ravi, Yanjing Li, and Frederic T Chong. *Proceedings of the ACM/IEEE International Symposium on Computer Architecture (ISCA '25)*, 2025.
- **[Triage: An Adaptive Parallel Window Decoding Scheduler for Real-time Fault-Tolerant Quantum Computation](https://arxiv.org/abs/2605.04459)**
  Jiahan Chen, Chenghong Zhu, Ge Bai, and Xin Wang. *arXiv preprint arXiv:2605.04459*, 2026.
- **[A Case for Elastic Quantum Error Correction Decoders](https://doi.org/10.1145/3767295.3803584)**
  Satvik Maurya, Abtin Molavi, Aws Albarghouthi, and Swamit Tannu. *Proceedings of the 21st European Conference on Computer Systems (EuroSys '26)*, 2026.

<a id="s3-2-4"></a>
#### Noise- and Side-Information-Aware Decoding

- **[Thresholds for topological codes in the presence of loss](https://doi.org/10.1103/physrevlett.102.200501)**
  Thomas M. Stace, Sean D. Barrett, and Andrew C. Doherty. *Physical Review Letters*, 2009.
- **[Erasure qubits: Overcoming the T_1 limit in superconducting circuits](https://doi.org/10.1103/physrevx.13.041022)**
  Aleksander Kubica, Arbel Haim, Yotam Vaknin, Harry Levine, Fernando Brandao, and Alex Retzker. *Physical Review X*, 2023.
- **[Coping with qubit leakage in topological codes](https://doi.org/10.1103/physreva.88.042308)**
  Austin G. Fowler. *Physical Review A*, 2013.
- **[Alibaba Cloud Quantum Development Platform: Surface code simulations with crosstalk](https://arxiv.org/abs/2002.08918)**
  Cupjin Huang, Xiaotong Ni, Fang Zhang, Michael Newman, Dawei Ding, Xun Gao, Tenghui Wang, Hui-Hai Zhao, et al. *arXiv preprint arXiv:2002.08918*, 2020.
- **[Improved quantum error correction using soft information](https://arxiv.org/abs/2107.13589)**
  Christopher A. Pattison, Michael E. Beverland, Marcus P. da Silva, and Nicolas Delfosse. *arXiv preprint arXiv:2107.13589*, 2021.
- **[Low overhead fault-tolerant quantum error correction with the surface-GKP code](https://doi.org/10.1103/prxquantum.3.010315)**
  Kyungjoo Noh, Christopher Chamberland, and Fernando G. S. L. Brandao. *PRX Quantum*, 2022.
- **[Correlated decoding of logical algorithms with transversal gates](https://doi.org/10.1103/physrevlett.133.240602)**
  Madelyn Cain, Chen Zhao, Hengyun Zhou, Nadine Meister, J. Pablo Bonilla Ataides, Arthur Jaffe, Dolev Bluvstein, and Mikhail D. Lukin. *Physical Review Letters*, 2024.
- **[Fast correlated decoding of transversal logical algorithms](https://arxiv.org/abs/2505.13587)**
  Madelyn Cain, Dolev Bluvstein, Chen Zhao, Shouzhen Gu, Nishad Maskara, Marcin Kalinowski, Alexandra A. Geim, Aleksander Kubica, et al. *arXiv preprint arXiv:2505.13587*, 2025.
- **[Logical Qubits with Erasure Conversion Using Metastable Neutral Atoms](https://doi.org/10.1038/s41567-026-03309-0)**
  Bichen Zhang, Genyue Liu, Guillaume Bornet, Sebastian P. Horvath, Pai Peng, Shuo Ma, Shilin Huang, Shruti Puri, et al. *Nature Physics*, 2026.
- **[Low-overhead transversal fault tolerance for universal quantum computation](https://doi.org/10.1038/s41586-025-09543-5)**
  Hengyun Zhou, Chen Zhao, Madelyn Cain, Dolev Bluvstein, Nishad Maskara, Casey Duckering, Hong-Ye Hu, Sheng-Tao Wang, et al. *Nature*, 2025.

<a id="s4"></a>
## Conclusion and Future Direction

<a id="s4-1"></a>
### Future Directions

- **[Transversal architecture for megaquop-scale quantum simulation with neutral atoms](https://doi.org/10.1103/j2fw-ccmy)**
  Refaat Ismail, I-Chi Chen, Chen Zhao, Ronen Weiss, Fangli Liu, Hengyun Zhou, Sheng-Tao Wang, Andrew Sornborger, et al. *PRX Quantum*, 2026.
- **[decoder-bench: Benchmarking Decoders for Quantum Error Correction](https://doi.org/10.1109/iiswc66894.2025.00032)**
  Satvik Maurya, Joshua Viszlai, Nithin Raveendran, Poulami Das, and Swamit Tannu. *Proceedings of the IEEE International Symposium on Workload Characterization (IISWC '25)*, 2025.
