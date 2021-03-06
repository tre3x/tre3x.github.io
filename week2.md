## Week 3 & 4
### Working on generation of synthetic dataset

In these two weeks, I worked on the generation of clips which either contains a hard-cut, a dissolve or no transition. These clips are synthetically generated by mix-matching different shots of MEP video dataset. In the previous version of the tool, this very apporoach was used to generate transition clips from the TRECVID video dataset. These clips were further used to train the soft-cut detection/validation module used in the later part of the tool. Clips from MEP video datasets were also used in the training set, but they were restricted to the natural shot boundaries, which resulted in their low contribution in the training set in terms of number. However, MEP BW video set consists of the type of films which the tool is aimed to process. Hence, these clips contribute an important role in the training set. To overcome this, in these weeks I wrote the code to generate clips synthetically generated by fusing two shots from the MEP videoset to create hard-cuts or dissolves. 


#### Hard-Cut Generation
For the generation of hard-cuts, the number of frames of transition was set to two, as because there were a lot of multi-frame hard cuts found in the dataset, possibly due to encoding reasons. The required number of frames of the result hard-cut snippet can be specified in a shell file, with the number of hard cuts needed in the dataset. The default number of frame is set to 100.

#### Dissolves Generation
For the generation of dissolves, the program needs to be specified the time(in seconds) of dissolve(only) between two shots. The default time of dissolve is 1 second, and the number of result dissolve snippets is set to 100. The number of dissolves needed can also be mentioned in the shell file located `/data/MEP_data/run.sh`

The transitions are generated by weighted average(pixel-wise) of frames iteratively. The more time progresses in the clip, the less weight the first shot frame gets and, the more the second shot frame gets. 

#### No-cut snippets generation
The generation of no cut snippets is straightforward, simply considering a single shots. The default number of frames in snippets containg no cuts is 100 and the number of snippets to be produced can be tweaked in the shell file located at `/data/MEP_data/run.sh`.\

The shots used in the mix-matching were seperated using the manual-annotation CSVs. However, it was noted that the manual annotations were not frame-accurate, and had an deflection of around 5-10 frames from the actual shot boundaries. These resulted in the presence of multiple cuts in the generated synthetic snippets. I was advised by my mentors to consider the midpoint of the shot boundaries as an anchor frame and expand about it in two directions to generate the shot snippets. The synthetic dataset was perfectly generated by using this technique to form shot snippets.

These synthetic clips will be used to form a dataset, which would be used to train a soft-cut detection/validation module. Also the adjacent shot boundary frame of the hard-cut snippets would be used to train a possible hard-cut detection module which works on a learnable threhsold mechanism.