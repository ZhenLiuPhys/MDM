# MDM
This is the model file written by Xing Wang, for our study https://arxiv.org/abs/2009.11287. Please cite this paper when you use them. 


1. The "Source" folder should be located at your output directory. Say, for example, you do "generate e+ e- > mu+ mu-", and then "output ee2mm". This will generate an output directory called "ee2mm", and the photon flux file will be in "ee2mm/Source/PDF/PhotonFlux.f".

2. The syntax depends on what kind of processes you want. If you only want the annihilation process through a s-channel EW bosons, then you can simply do "mu+ mu- > dm dm" and don't need to modify lpp1/lpp2 at all. 
But when you want to include photon-photon fusion processes, like "mu+ mu- > mu+ mu- chip1 chip1~", with VBF-like topology, and you want the process to be inclusive (i.e. you don't require the outgoing muons to be tagged), then you cannot simply tell Madgraph to do "generate mu+ mu- > mu+ mu- chip1 chip1~", because such process contains large logs due the tiny muon mass and also not numerically stable. The right way is to do what we do with jets at LHC - we treat photons as initial "partons" and use PDFs. And the syntax should be "generate a a > chip1 chip1~", set lpp1=lpp2=3, and modify the "PhotonFlux.f" file. Madgraph will compute the partonic process "a a > chip1 chip1~" and then convoluted with photon PDFs.

For example, in our paper 2009.11287, we included 4 processes for the mono-photon channel, listed in Eq. 3.2-3.5. And the corresponding syntax is 
"generate mu+ mu- > dm dm a" with lpp1=lpp2=0 --- Eq. 3.2
"generate a a > dm dm a" with lpp1=lpp2=3 --- Eq. 3.3
"generate a mu- > dm dm a vm" with lpp1=3 and lpp2=0 --- Eq. 3.4a
"generate mu+ a > dm dm a vm~" with lpp1=0 and lpp2=3 --- Eq. 3.4b
"generate mu+ mu- > dm dm a vm vm~" with lpp1=0 and lpp2=0 --- Eq. 3.5
