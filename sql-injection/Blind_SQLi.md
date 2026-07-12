## SQLi - blind SQL injection (conditional responses)

### what I learned
blind injection = no results visible, extract data by reading behavior changes
query runs but response doesn't show output - infer truth from "welcome back" appearing or not

### the workflow
1. find the behavioral difference:
   - true condition → response length 5549 (welcome back appears)
   - false condition → response length 5580 (no welcome back)
   
2. use that signal to extract character by character:
   - position 1, test 'a', 'b', 'c'... until response is 5549
   - position 2, test 'a', 'b', 'c'... until response is 5549
   - repeat for all positions
   
3. cluster bomb in intruder: all position/character combos at once
   - slow as hell in community edition (2h+ for 720 requests)
   - scan results for response length matching your "true" signal

### result
extracted 20-character password: j8jnmq49u246vsjx1mtn
without ever seeing a single character directly on the page

### key lesson
community edition is a learning tool, not production. custom scripts with threading would fire 10+ requests in parallel instead of sequential, cutting time from 2h to 10 mins
