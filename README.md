Simple Python Search Spider, Page Ranker, and Visualizer

This is a set of programs that emulate some of the functions of a search engine. They store their data in a SQLITE3 database named 'spider.sqlite'. This file can be removed at any time to restart the process.

You should install the SQLite browser to view and modify the databases from:

http://sqlitebrowser.org/

This program crawls a web site and pulls a series of pages into the database, recording the links between pages.

Note: Windows has difficulty in displaying UTF-8 characters in the console so for each console window you open, you may need to type the following command before running this code:

Win: del spider2.sqlite Win: spider2.py

Enter web url or enter: https://www.microsoft.com/ ['https://www.microsoft.com'] How many pages:50 431 https://www.microsoft.com/en-us/locale (114309) 182 1540 https://www.microsoft.com/fi-fi/surface/business/surface-laptop-intel-7th-edition (542423) 264 1904 https://www.microsoft.com/en-us/microsoft-365/blog/audience/personal-and-family/?ep_date_filter=last-6-months (275041) 151 720 https://www.microsoft.com/en-in/ai/government/public-finance (259088) 114

In this sample run, we told it to crawl a website and retrieve fifty pages. If you restart the program again and tell it to crawl more pages, it will not re-crawl any pages already in the database. Upon restart it goes to a random non-crawled page and starts there. So each successive run of spider1.py is additive.

You can have multiple starting points in the same database - within the program these are called "webs". The spider chooses randomly amongst all non-visited links across all the webs.

If you want to dump the contents of the spider2.sqlite file, you can run spdump.py as follows:

Mac: python3 Spdump.py Win: Spdump.py

(5, 0.10480434426547292, 0.16754370469222735, 1542, 'https://www.microsoft.com/fi-fi/surface/business/surface-laptop-6') (5, 1.0, 0.16754370469222735, 1540, 'https://www.microsoft.com/fi-fi/surface/business/surface-laptop-intel-7th-edition') (5, 0.6947882023275631, 0.9930182050277621, 672, 'https://www.microsoft.com/en-us/microsoft-365-copilot/business/onboarding')
(5, 0.597198841843581, 0.723873203981738, 295, 'https://www.microsoft.com/en-in/industry/manufacturing/microsoft-cloud-for-manufacturing') (5, 0.19586481876606304, 0.22389802389738012, 130, 'https://www.microsoft.com/en-us/security/business/ai-machine-learning/microsoft-security-copilot') 150 rows.

This shows the number of incoming links, the old page rank, the new page rank, the id of the page, and the url of the page. The Spdump.py program only shows pages that have at least one incoming link to them.

Once you have a few pages in the database, you can run Page Rank on the pages using the sprank.py program. You simply tell it how many Page Rank iterations to run.

Mac: python3 sprank.py Win: Sprank1.py

How many iterations:4 1 5.777342075061389e-16 2 6.752589355254377e-16 3 5.440853247392463e-16 4 5.281163634261446e-16 [(1, 11.390997257372309), (9, 9.107765434699957), (22, 1.9622886394193546), (23, 2.294958412149092), (51, 12.032093461042484)]

You can dump the database again to see that page rank has been updated:

Mac: python3 Spdump.py Win: Spdump.py

(5, 0.16754370469206908, 0.1675437046920697, 1542, 'https://www.microsoft.com/fi-fi/surface/business/surface-laptop-6') (5, 0.16754370469206908, 0.1675437046920697, 1540, 'https://www.microsoft.com/fi-fi/surface/business/surface-laptop-intel-7th-edition') (5, 0.9930182050277785, 0.993018205027779, 672, 'https://www.microsoft.com/en-us/microsoft-365-copilot/business/onboarding')
(5, 0.7238732039816005, 0.7238732039816004, 295, 'https://www.microsoft.com/en-in/industry/manufacturing/microsoft-cloud-for-manufacturing') (5, 0.22389802389736696, 0.22389802389736752, 130, 'https://www.microsoft.com/en-us/security/business/ai-machine-learning/microsoft-security-copilot') 150 rows.

You can run Sprank1.py as many times as you like and it will simply refine the page rank the more times you run it. You can even run Sprank.py a few times and then go spider a few more pages with spider1.py and then run Sprank1.py to converge the page ranks.

If you want to restart the Page Rank calculations without re-spidering the web pages, you can use spreset.py

Mac: python3 spreset.py Win: spreset.py

All pages set to a rank of 1.0

Mac: python3 Sprank1.py Win: Sprank1.py

How many iterations:50 1 0.02176414577925984 2 0.007949427526212529 3 0.003860079415941765 4 0.0016034443267159374 5 0.0006312774432835443 ......... 43 2.472088363542449e-13 44 1.3910429125165165e-13 45 7.982284924370231e-14 46 4.38428783384615e-14 47 2.511670475863888e-14 48 1.438689350301072e-14 49 8.194453487149838e-15 50 4.232820334724559e-15 [(1, 11.390997257372765), (9, 9.107765434700273), (22, 1.962288639419437), (23, 2.2949584121491835), (51, 12.03209346104294)]

For each iteration of the page rank algorithm it prints the average change per page of the page rank. The network initially is quite unbalanced and so the individual page ranks are changing wildly. But in a few short iterations, the page rank converges. You should run Sprank1.py long enough that the page ranks converge.

If you want to visualize the current top pages in terms of page rank, run spjson.py to write the pages out in JSON format to be viewed in a web browser.

Mac: python3 Spjson.py Win: Spjson.py

Creating JSON output on spider.js... How many nodes? 50 Open force.html in a browser to view the visualization

You can view this data by opening the file force.html in your web browser.
This shows an automatic layout of the nodes and links. You can click and drag any node and you can also double click on a node to find the URL that is represented by the node.

If you rerun the other utilities and then re-run spjson.py - you merely have to press refresh in the browser to get the new data.
