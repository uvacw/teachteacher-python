# Teachingtips

This document contains some tips from experience to help you avoiding the most common pitfalls when getting started to teach with Python.


## Avoiding technical problems
- Make everyone install (and test!) the environment *before* class starts. Avoid having to deal with "I can't open the notebook!"- or "It says 'module not found'!"-questions during class.
- **The first session is crucial.** Expect that during the first session, there will be students with technical problems. It is best to teach this session with two teachers, such that one can deal with individual problems and the other can continue with the rest of the group.
- Be aware that even though Python is largely platform-independent (yeah!!!), there can be subtle differences between using it on typical unix-based systems (MacOS or Linux) versus Windows. Think of `/home/damian/pythoncourse` vs `C:\\Users\\damian\\pythoncourse` (note the double (!) backslash!), but also about some modules that may have different external requirements (recent experience: try `pip install geopandas` on different systems!)
- You can consider pointing students to [Google Colab](https://colab.research.google.com/) as a fallback option if they cannot get things to work.


## Grading and rules for assignments
- Make clear from the beginning that it is fine, even encouraged, to get ideas from sites like https://stackoverflow.com . Emphasize, though, that copy-pasting code and presenting it as own work is considered plagiarism, just like in written assignments. A simple comment line like `# the following cell is [copied from/adapted from/inspired by] URL` is enough to prevent this. Document this rule, for instance in the course manual.


