---
layout: post
title: "Confusion Matrix - Alternative Visualization"
description: "Proposed alternative visualization to confusion matrices for classification results."
tags: [visualization, python, classification, confusion, matrix]
image:
    feature: piePlots.jpg
---

When performing multi-class classification, confusion matrices do a good job at presenting the results 
while preserving all information: % correct classification accuracy, % misclassifications and misclassification 
classes for each predicted class. Its when the number of classes gets beyond ~5 classes that these visualizations 
start to become inappropriate. The matrices become too large to be presented anywhere; whether on a presentation 
slide or figure in a manuscript. The issue is further amplified when we have hierarchical classification, where we 
want to show inherited (mis)classifications down a tree. 

<!-- more -->

When visualizing data, its always a matter of balance between information and simplicity. In my case, I'm interested 
in the relative proportion(s) of misclassifications of a target class and into which classes the misclassifications 
occured. Since I'm performing hierarchical classification, I'm also interested in grouping the classes to be able to 
determine misclassification classes at the upper hierarchy level with a quick glance. High accuracy values are not a 
priority so I came up with a semi-quantitative visualization which I'm calling "confused pie plots":

<figure>
	<img src="/images/piePlotsDemo.jpg" alt="">
	<figcaption>Generated figure with demo data. You can clearly see that child node 'a' for parent class 'A' (represented 
	by pie plot Aa for short) was misclassified overall into a child node belonging to parent E. Similarly for child j parent D (Dj) 
	which was misclassified into class D (and child h). Other misclassifications for child nodes in parent class A were misclassified 
	between each other within the same parent class.</figcaption>
</figure>

Yes, customized pie charts. So we have the inner ellipse showing the expected target class, and the outer ellipse represents the 
predicted classes. Rows represent child nodes belonging to the same parent (column). Its relatively straightforward to 
see where misclassifications occurred. Obviously less so when the color scale becomes limiting with a very large number 
of classes. But even then, misclassifications at the parent nodes is still easy to see with a specific color-scale assigned 
to the parents (tested it with up to 35 classes - works quite well).

Here's the code to generate this (or fork it on GitHub). Requires a confusion matrix in csv as input, with target classes 
as rows, and predicted classes as columns. Labels should be first column and first for parent classes and second column, 
second row for child classes.

### Generate Figure ([fork on GitHub](https://github.com/dterg/confused-pie-plots))

{% highlight python %}
'''
Confused pie plots

This script is free software: you can redistribute it and/or modify it under the terms of the GNU General Public
License as published by the Free Software Foundation, either version 3 of the License, or any later version.

This script utilizes matplotlib and numpy libraries - BSD licensed software.

This script is distributed in the hope of being useful, but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License along with this script. If not, see
http://www.gnu.org/licenses

Author: Dieter Galea, 2016
'''

import matplotlib
matplotlib.use('Tkagg')
import matplotlib.pyplot as plt
from matplotlib.gridspec import GridSpec
import numpy as np
import matplotlib.colors as colors
import random
import pandas as pd

# import confusion matrix
fullConfMat = pd.read_csv('confMat.csv', delimiter=',', header=None)
confMat = np.array(fullConfMat.ix[2:,2:])

# get class names
childNames = fullConfMat[1][2:].tolist()
parentNames = fullConfMat[0][2:].tolist()

# calculate number of classes/parents
nChildren = len(childNames)
uniqueParents = np.unique(parentNames)
nCols = np.shape(uniqueParents)[0]
nRows = 1
idx = []

# put labels into a dictionary
parentDict = dict((el,0) for el in parentNames)
for iParent, parent in enumerate(uniqueParents):
    for i, j in enumerate(parentNames):
        if j == parent:
            idx.append(i)
    parentDict[parent] = idx
    iUpperLvlClasses = [childNames[i] for i in idx]
    iLevelnClasses = np.shape(np.unique(iUpperLvlClasses))[0]
    idx = []

    # store the maximum number of classes for a parent
    if iLevelnClasses > nRows:
        nRows = iLevelnClasses

# calculate number legend rows needed
legendRowsNeeded = int(np.ceil(float(nChildren) / float(nCols)))
totalRows = nRows + legendRowsNeeded

# create a grid space
heightRatios = []
[heightRatios.append(3) for x in range(0,nRows)]
[heightRatios.append(1) for x in range(0,legendRowsNeeded)]
the_grid = GridSpec(totalRows, nCols, height_ratios=heightRatios)
nColors = nChildren
cmap = plt.cm.gist_ncar
colors = cmap(np.linspace(0.,1.,nColors))

fig = plt.figure(facecolor='white')
ax = fig.gca()

iCounter = 0
speciesCounter = 0
iRow = totalRows - legendRowsNeeded

# plot each class for each parent
for iParent, parent in enumerate(uniqueParents):
    childrenIdx = parentDict[parent]
    for jChild, child in enumerate(childrenIdx):
        speciesCounter += 1
        spName = childNames[child]
        plt.subplot(the_grid[jChild,iParent], aspect=1)
        if jChild == 0:
            plt.text(-1, 1.2, parent[0:9]+'.', fontsize=10)
        sliceSize = confMat[child,]
        if confMat[child, child] == 100:
            predictedSlices = plt.pie([100],
                            colors = colors[[child,]],
                            shadow=False,
                            startangle=90,
                            radius=1)
        else:
            predictedSlices = plt.pie(sliceSize,  # data
                colors=colors,  # array of colours
                shadow=False,   # disable shadow
                startangle=90,  # starting angle
                radius=1)
        for wedge in predictedSlices[0]:
            wedge.set_linewidth(0.1)

        actualSlices = plt.pie([100],
                colors=colors[[child,]],
                shadow=False,
                startangle=90,
                radius=0.4)

        # abbreviate name - this applies for bacterial species where naming is 'Parent child'
        # and this abbreviates to 'P. child'
        Abv = parent[0]
        AbvName = Abv + '. ' + spName

        # check if color is dark
        totalColor = 0.299 * colors[child, 0] + 0.587 * colors[child, 1] + 0.114 * colors[child, 2]
        if totalColor > 0.3:
            plt.text(-0.2, -0.1, AbvName[0]+AbvName[3], color='black', fontsize=10)
        else:
            plt.text(-0.2, -0.1, AbvName[0] + AbvName[3], color='white', fontsize=10)

        # draw legend
        if iCounter > nCols-1:
            iCounter = 0
            iRow += 1
        plt.subplot(the_grid[iRow, iCounter], aspect = 1)

        legendDots = plt.pie([100],
                               colors=colors[[child, ]],
                               shadow=False,
                               startangle=90,
                               radius=0.8)

        plt.text(1.2,-0.1,AbvName, fontsize=10) ### 4.5 adjust depending on resolution

        iCounter += 1

mng = plt.get_current_fig_manager()
#mng.resize(*mng.window.maxsize())
plt.show()
plt.savefig('Figure.tiff', format='tiff', dpi=320)

{% endhighlight %}
