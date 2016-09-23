---
layout: post
title: Visualizing Hierarchical Trees - XML and JSON Generator
description: "Generating XML and JSON for D3 and iTol."
tags: [MATLAB, JSON, XML, D3, Hierarchy, Trees]
image:
    feature: d3hierarchytree.jpg
    credit: mbostock
    creditlink: https://bl.ocks.org/mbostock/4063550
---

Let’s start off with a quick script. I’m sure you’ve heard of the [D3 library](https://github.com/d3/d3). If not, it’s a highly versatile JavaScript library used for visualizing data. It has been used to generate some already-familiar visualization (e.g. correlation matrix) with added functionality and interactivity, or at least easier to generate, and new concepts – too many to choose from. The [example gallery]( https://github.com/d3/d3/wiki/Gallery) alone is quite inspiring. Some [others]( http://www.enpicom.com/visual-lab/tcga-exploration/embed/) are cool visualizations but not sure if they suit the purpose. Anyway, enough mumbling.

I’ve recently needed to visualize a hierarchical tree, so I customized the [Radial Reingold-Tilford Tree](http://bl.ocks.org/mbostock/4063550). This is based on a JSON file which I wanted to automatically generate from a MATLAB cell array with bacterial taxonomic classification; where columns represent tree levels and rows the different children/nodes at the bottom-most level. 
It’s a very quick-and-dirty script which first determines the unique bottom-level child nodes and recursively finds nodes with a common ancestor/parent node. 
<!-- more -->

### Generate JSON ([fork on GitHub](https://github.com/dterg/generate-JSON-XML))

{% highlight ruby %}
function createJSON(ytaxa)

%%% open an empty JSON file
global fileID
fileID = fopen('generatedJSON.json','wt');

%%% write the root class
fprintf(fileID, '{\n"name": "root", \n"children": [\n');

[uniqueSp,uniqueIdx,~] = unique(ytaxa(:,end));

%%% remove duplicate species
ytaxa = ytaxa(uniqueIdx,:);
indent = '';
determineUnique(ytaxa,1, indent);
fclose('all');
end

function determineUnique(ytaxa,iLevel, indent)
global fileID
%%% unique classes in the current level
[uniqueSp, uniqueIdx,~] = unique(ytaxa(:,iLevel));

%%% for each class determine next level unique classes
for iSp = 1:length(uniqueSp)
    currSp = uniqueSp(iSp);
    idx = find(strcmpi(ytaxa(:,iLevel), currSp{:}));
    %%% if current species is the last one for the current level
    %%% close branch without comma
    if iSp == length(uniqueSp)
        closeBranch = true;
    else
        closeBranch = false;
    end
    %%% have we reached the bottom level yet?
    if iLevel == size(ytaxa,2)
        indent = createIndentation(iLevel);
        if iSp == length(uniqueSp)
            fprintf(fileID, '%s{"name": "%s"}\n', indent, currSp{:});
            indent = createIndentation(iLevel-1);
            fprintf(fileID, '%s]\n%s}', indent, indent);
        else
            fprintf(fileID, '%s{"name": "%s"},\n', indent, currSp{:});            
        end
        continue
    else
        indent = createIndentation(iLevel);
        fprintf(fileID, '%s{\n%s"name": "%s",\n%s"children": [\n', indent, indent, currSp{:}, indent);
        determineUnique(ytaxa(idx,:), iLevel+1, indent)
        if closeBranch
            fprintf(fileID, '\n%s]\n%s}\n',indent,indent);
        else
            fprintf(fileID, ',\n');
        end
    end
end

end

function [indent] = createIndentation(iLevel)
indent = [''];
for i = 1:iLevel
    indent = [indent, '   '];
end
end
{% endhighlight %}


Alternatively, I came across [iTol (Interactive Tree Of Life)](http://itol.embl.de/) “an online tool for the display, annotation and management of phylogenetic trees”. It’s a very easy to use point-and-click tool that I found quite convenient (although can be a bit buggy at times). It takes XML as input so I’ve adjusted the previous script for this. 


### Generate XML ([fork on GitHub](https://github.com/dterg/generate-JSON-XML))

{% highlight ruby %}

function createXML(ytaxa)

%%% open an empty xml file
global fileID
fileID = fopen('generatedXML.xml','wt');

%%% write the default opening txt
fprintf(fileID, '<?xml version="1.0" encoding="UTF-8"?>\n');
fprintf(fileID, '<phyloxml xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"\n');
fprintf(fileID, '   xsi:schemaLocation="http://www.phyloxml.org http://www.phyloxml.org/1.10/phyloxml.xsd"\n');
fprintf(fileID, '   xmlns="http://www.phyloxml.org">\n');
fprintf(fileID, '   <phylogeny rooted="true">\n');
[uniqueSp,uniqueIdx,~] = unique(ytaxa(:,end));

%%% remove duplicate species
ytaxa = ytaxa(uniqueIdx,:);
indent = '   ';
determineUnique(ytaxa,1, indent);
fprintf(fileID, '   </phylogeny>\n');
fprintf(fileID, '</phyloxml>');
fclose('all');
end

function determineUnique(ytaxa,iLevel, indent)
global fileID
%%% unique classes in the current level
[uniqueSp, uniqueIdx,~] = unique(ytaxa(:,iLevel));

%%% for each class determine next level unique classes
for iSp = 1:length(uniqueSp)
    currSp = uniqueSp(iSp);
    idx = find(strcmpi(ytaxa(:,iLevel), currSp{:}));
    %%% if current species is the last one for the current level
    %%% close branch without comma
    if iSp == length(uniqueSp)
        closeBranch = true;
    else
        closeBranch = false;
    end
    %%% have we reached the bottom level yet?
    if iLevel == size(ytaxa,2)
        spaceIdx = strfind(currSp{:}, ' ');
        spName = [currSp{1}(1), '.', currSp{1}(spaceIdx:end)];
        indent = createIndentation(iLevel);
        fprintf(fileID,'%s<clade>\n', indent);
        indent = createIndentation(iLevel+1);
        fprintf(fileID, '%s<name>%s</name>\n', indent, spName);
        indent = createIndentation(iLevel);
        fprintf(fileID, '%s</clade>\n', indent);
        indent = createIndentation(iLevel-1);
        if closeBranch
            fprintf(fileID, '%s</clade>\n',indent);
        else
            fprintf(fileID, '\n');
        end
        continue
    else
        indent = createIndentation(iLevel);
        fprintf(fileID, '%s<clade>\n', indent);
        determineUnique(ytaxa(idx,:), iLevel+1, indent)
        indent = createIndentation(iLevel-1);
        if closeBranch
            if iLevel == 1
            else
                fprintf(fileID, '%s</clade>\n',indent);
            end
        else
            fprintf(fileID, '\n');
        end
    end
end

end

function [indent] = createIndentation(iLevel)
indent = ['   '];
for i = 1:iLevel
    indent = [indent, '   '];
end
end
{% endhighlight %}


Haven’t had the time to write this in python, but should be very straightforward. If you do, let me know through GitHub and I’ll be happy to include it.



