# How Does Query, Weight and Scorer Work?
*this is work in progress*



# References

* [Doug Turnbull's Buld your own Custom Lucene query and scorer](https://opensourceconnections.com/blog/2014/01/20/build-your-own-custom-lucene-query-and-scorer/)

# Appendix : Background
The Pensieve Translation Memory system needs to know how good a match is, we need a scoring system that tells me whether the match is 100% (exact match), or similar but not exact (less than 100%).

Lucene's native scoring doesn't work this way. 
It gives relative scores of hits to sort the hits, but the score values themselves do not mean much. 
If you have a very short document and even if the query terms include all the words in the document, the score can be 0.7 or it can be 3.0, depending on what kind of Query is used, and the contents of other documents in the index (because the scoring algorithm uses the index statistics.)

That is why we need to tweek the Lucene's scoring system. 
The original Pensieve code has custom Query, Weight and Scorer classes. 
But it was found that the porting this code was rather difficult because the Lucene APIs it uses have changed a lot in 5 major releases.

Doug's blog article warns against customizing Query, Weight, and Scorer and recommends customizing Similarity first. 
Combined with difficulty in porting, customizing Similarity was first attempted. 
But I soon found out this way does not work. 
The score calculation within a Similarity class can only look at terms in the query. 
I could make it so that the score=1.0 if all the terms in the queries were found in a document. 
For examle, if there is a document that has "The quick brown fox jumps over the lazy dog." in a search field and I could make it so that the query "The quick brown fox jumps over the lazy dog." generates a score of 1.0.
But the problem is that the query "quick brown fox" also generates the score of 1.0, because all the terms are found in the documentt.
Because the Similarity class does not have access to the document (as far as I can tell), there is no way to tell that all the tokens from the document are in the query.

So I gave up using a custom Similarity, and went back to the effort of porting the custom Query, Weight and Scorer.
