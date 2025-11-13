# Finding Hidden urls in a website
To find hidden URLs, we will use a tool called **dirb**. This tool uses a brute-force approach, by taking a list of potential page names and testing one by one if they exist in your website. This approach works because people use predictable names a lot of times.
You open your terminal and run the command **dirb folowed by the url of the website**
```
dirb http://fakebank.thm
```
