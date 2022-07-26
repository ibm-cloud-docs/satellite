### What must be peer reviewed?
Every change in the docs must be peer reviewed, unless you are JUST fixing a typo. When you add or change a sentence, you must get a peer review.
If your change is part of a larger overhaul or a bigger change in the docs, you can merge the content to staging without a peer review and inform the peer reviewer once you are done with the change and ready to have it peer reviewed. 
Important: Peer reviews do not replace technical reviews. Make sure to have your content reviewed by dev as well to ensure technical accuracy. 


### What is included in a peer review? 

**Content:** 
- [ ] Does the topic/ section make sense and is worded in a way that a user can understand it? 
- [ ] Does the topic/ section seem to be consistent in style, look and feel with other sections in this topic/ other topics? 
- [ ] Do the provided steps work? Are steps/ explanations missing? (If possible, steps should be tested for new topics, and when new steps are added to existing topics)
- [ ] Are steps/ actions "hidden" in steps?
- [ ] Is the happy path, the why or the most common use case documented? 
- [ ] Are there any typos? Is the topic grammatically correct? 
- [ ] Is the topic/ section scannable or can you replace things with other elements, such as tables, lists, codeblocks, etc. ? (-> look for long paragraphs of text, long sentences, sentences with a lot of commas)
- [ ] Does the topic/ section meet IBM style guidelines? (e.g. look for allowed terms, present tense, no use of "there are", "may", or missing nouns after "this", referring to a noun as "it", etc. )
- [ ] Do headings match what you find in the topic/ section?
- [ ] Is important content "hiding" in H3s or in long paragraphs of text?
- [ ] Is the topic/ section written with a conversational style?
- [ ] Are service name conrefs used?
- [ ] Are codeblocks, pre, and screen tags used correctly? (e.g. YAML files in codeblocks, commands in pre, outputs in screen)

**In Addition, if peer review is done in staging:** 

- [ ] Does the format look ok? (e.g. indention of steps, numbering in lists, codeblocks, tables, colors, spacing)
- [ ] Do conrefs appear correctly?
- [ ] Does the TOC look ok?
- [ ] Do links work and point to the right topic? 
- [ ] Do you see any misplaced staging/ prod tags or conrefs?
- [ ] Is there enough space between topics/ steps? 
- [ ] Do images display correctly?


**Images: **

- [ ] Was the IBM Plex font used? 
- [ ] Are the right service icons used? Can text be replaced with a service icon?
- [ ] Is the flow comprehensive? 
- [ ] Is the image self-explanatory? (as much as it can, given that we show complex technical content)




### What is NOT included in a peer review and must be completed by the writer? 
- Does the formatting looks ok in the docs framework (if peer review was done in a PR) 
- Check for broken links (if peer review was done in a PR) 
- Acrolinx testing and resolving of issues
- Making sure that images are accessible
- Making sure that images are ok to be translated (see guidelines)
