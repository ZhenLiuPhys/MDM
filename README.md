# MDM
This is the model file written by Xing Wang (xingwang1990@gmail.com), for our study https://arxiv.org/abs/2009.11287. Please cite this paper when you use them. 
If you have any questions, please contact Xing Wang (xingwang1990ATgmail.com), and Zhen Liu (zliuphysATumn.edu)

UFO model files for Madgraph.
f2d_n-plet_UFO: Dirac SU(2) doublet - neutral component chiN (chiN~), singly charged chip1 (chip1~)

f3m_n-plet_UFO: Majorana SU(2) triplet - neutral component chiN, singly charged chip1 (chip1~)

f5m_n-plet_UFO: Majorana SU(2) 5-plet - neutral component chiN, singly charged chip1 (chip1~), doubly charged chip2 (chip2~)

f7m_n-plet_UFO: Majorana SU(2) 7-plet - neutral component chiN, singly charged chip1 (chip1~), doubly charged chip2 (chip2~), triply charged chip3 (chip3~)

---------------------------------------------------
Instructions
---------------------------------------------------


As for the process generation, we used photon-initiated processes together with the photon PDF based on Madgraph implementation with the following modifications: 
- Madgraph supports photon PDF from the initial electron or positron by setting lpp1 or lpp2 = 3, and the formula is described in arXiv:hep-ph/9310350. To have the muon version, you need to modify "Source/PDF/PhotonFlux.f" in your work directory and replace the electron mass with muon mass. 
- And for scale (which is the integration bound of q^2 as in hep-ph/9310350,up to a minus sign that flips what they call min/max), you can use the Madgraph built-in dynamical scale with fixed_fac_scale=False and dynamical_scale_choice=4, which uses sqrt(s_hat) as the scale; and then set scalefact=0.5 to make it down to sqrt(shat)/2.

The photon initiated signal processes are less relevant when setting the upper bound on the DM mass, because for heavy masses the VBF topology is not very efficient in terms of using the collision energy. This can be seen from Fig. 3a in our paper 2009.11287, where the annihilation processes are dominant when the DM mass is large. On the other hand, when the mass is small (compared to sqrts), the photon-initiated processes do have sizable contribution, especially for higher multiplets at higher energies.


1. The "Source" folder should be located at your output directory. Say, for example, you do "generate e+ e- > mu+ mu-", and then "output ee2mm". This will generate an output directory called "ee2mm", and the photon flux file will be in "ee2mm/Source/PDF/PhotonFlux.f".

2. The syntax depends on what kind of processes you want. If you only want the annihilation process through a s-channel EW bosons, then you can simply do "mu+ mu- > dm dm" and don't need to modify lpp1/lpp2 at all. 
But when you want to include photon-photon fusion processes, like "mu+ mu- > mu+ mu- chip1 chip1~", with VBF-like topology, and you want the process to be inclusive (i.e. you don't require the outgoing muons to be tagged), then you cannot simply tell Madgraph to do "generate mu+ mu- > mu+ mu- chip1 chip1~", because such process contains large logs due the tiny muon mass and also not numerically stable. The right way is to do what we do with jets at LHC - we treat photons as initial "partons" and use PDFs. And the syntax should be "generate a a > chip1 chip1~", set lpp1=lpp2=3, and modify the "PhotonFlux.f" file. Madgraph will compute the partonic process "a a > chip1 chip1~" and then convoluted with photon PDFs.

For example, in our paper 2009.11287, we included 4 processes for the mono-photon channel, listed in Eq. 3.2-3.5. And the corresponding syntax is 
"generate mu+ mu- > dm dm a" with lpp1=lpp2=0 --- Eq. 3.2
"generate a a > dm dm a" with lpp1=lpp2=3 --- Eq. 3.3
"generate a mu- > dm dm a vm" with lpp1=3 and lpp2=0 --- Eq. 3.4a
"generate mu+ a > dm dm a vm~" with lpp1=0 and lpp2=3 --- Eq. 3.4b
"generate mu+ mu- > dm dm a vm vm~" with lpp1=0 and lpp2=0 --- Eq. 3.5
