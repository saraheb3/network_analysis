==========================================
# communities-network_analysis
==========================================
# This code implements the method presented in Efficient method for estimating the number of communities in a network
# M. A. Riolo, G. T. Cantwell, G. Reinert, and M. E. J. Newman, "Efficient method for estimating the number of communities in a network," Physical Review E, vol. 96, no. 3, p. 032310, 09/14/ 2017.

This code, written in C by Riolo, Cantwell, Reinert, and Newman, calculates the number of communities in an
undirected network using the maximum likelihood method of Riolo, Cantwell,
Reinert, and Newman.

Files:
  communities.c: the main code file
  network.h, readgml.c, readgml.h: utility code
  Makefile: a Linux makefile for compiling the code
  
  lesmis.gml: an example input file, the network of character interactions
    from the novel "Les Miserables" analyzed in the paper
  hypertext.gml: Hypertext 2009 dynamic contact network, which maps the contact network during the Hypertext 2009 conference
    available from the SocioPatterns collaboration for download at http://www.sociopatterns.org/.
    L. Isella, J. Stehlé, A. Barrat, C. Cattuto, J.-F. Pinton, and W. Van den Broeck, "What's in a crowd? Analysis of face-to-face    behavioral networks," Journal of Theoretical Biology, vol. 271, no. 1, pp. 166-180, 2011/02/21/ 2011.
  Eurosis.gml: EuroSiS web mapping study dataset, which maps interactions between Science in Society actors on the Web of 12 European countries.
    D. V. Welden, in FUBUTEC’2004: 1st future business technology conference, EUROSIS, 2004, pp. 55–59.
     

The program reads networks in GML format.  To analyze a network, after
compiling the code, feed the GML file to stdin thus:

communities < lesmis.gml

The output consists of a series of Monte Carlo samples like this:

0 18 15.4969 -946.068
1 15 13.0113 -891.124
2 12 10.5542 -829.292
3 12 9.9061 -814.68
4 11 9.1001 -794.695

The four columns are Monte Carlo sweep number, number of groups k,
effective number of groups k_eff (as defined in the paper) and
log-likelihood, the latter accurate to within overall additive and
multiplicative constants.

Normally you'll want to discard some number of initial Monte Carlo sweeps
while the algorithm comes to equilibrium.  Discarding the first 1000 sweeps
is typical, but if you want to be certain about it, make a plot of the
fourth column -- it's usually pretty obvious from the plot of
log-likelihood when the algorithm has come to equilibrium.  Then you can
make a histogram of the values of k from that point onward and you'll get
an estimate of the probabilities of different values.

The program contains a small number of hard-coded constants, all defined
near the start of the file:

K: Maximum number of groups.  Smaller values will give faster
  equilibration, but the program will never return values of k larger than
  K, so K must be set large enough for your particular network.  Currently
  set to 25.

MCSWEEPS: Number of Monte Carlo sweeps.  Larger numbers may be needed for
  larger networks.  Currently set to 10,000, which is fine for smaller
  networks, but we have used values up to 100,000 in some cases.

SAMPLE: Frequency with which results are printed out.  Currently set to 1,
meaning the results are printed out after every sweep.


OLDER CODE:

Also included here is the program estimate.c, which implements an older
(and slower) variant of the method, as described in the author's earlier paper,
Newman and Reinert, Phys. Rev. Lett. 117, 078301 (2016).  This version of
the code is included solely for the sake of completeness.  The newer
algorithm in communities.c supersedes the older one, being both faster and
more accurate.
