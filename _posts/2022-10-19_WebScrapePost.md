---
layout: post
title:  "Zillow Data"
date:   2022-10-19
author: Zack Driscoll
description: Run code to webscrape zillow and understand the housing market better
image: /assets/images/zillow.jpg
---
# Getting House Data From Zillow
###### by Zack Driscoll

![Main](https://raw.githubusercontent.com/zadriscoll/stat386-projects/main/assets/images/zillow.jpg)
### Inspiration
This topic is a no brainer for me. My wife is a realtor, my dad builds custom homes, and just this year my wife and I purchased our first property. A key motivator in real estate is making sure you are getting the best deal. The data found in this article is not unheard of. In fact, you can easily find all of this data on Zillow. The real value is that from this blog post, you will be equiped to walk away from here with code in hand and the ability to quickly analyze all properties in any given city. The ease of use and speedy calculations is what makes this amazing. Real estate analysts would love this type of set up. 

### First Things First:
Before getting too head deep, I needed to understand what Zillow looks like and how I would justify the ethical side of things. Ethically, this is very sound. No personal information is attached and all I am doing is organizing already accessible data to the public.

I wanted to see what I was dealing with layout wise, so I navigated to Zillow and ran a quick search for homes in SLC, UT.
![Zillowweb](https://raw.githubusercontent.com/zadriscoll/stat386-projects/main/assets/images/zillowoutline.JPG)

Upon clicking on the inspect tool, this is what I was greeted with:
![inspecttool](https://raw.githubusercontent.com/zadriscoll/stat386-projects/main/assets/images/zillowinspect.JPG)
These are some basic layouts of what I was dealing with. 

### Research, Don't Re-invent the Wheel
When it comes to doing things that are hard, someone most likely has already done it. Don't stress about re-inventing the wheel. Use good resources to propel you forward. 
I found some online code chunks that really served as the frame work for my webscraping:
For a basic layout this is what I found:
* [Zillow Scraping](https://blog.devgenius.io/scraping-zillow-with-python-and-beautifulsoup-bbc7e581c218)
* [Zillow Scraping 2](https://stackoverflow.com/questions/69024599/scraping-data-from-zillow-com-using-beautifulsoup)

### Off to the Races
From my extensive research, I was ready to go! I added some for loops for ease of use and efficiency.
```python
city = 'saltlakecity-ut/' #*****change this city to what you want!!!!*****
data_list = []
urls = []
for page in range(1,20):
    if page == 1:
        url = 'https://www.zillow.com/homes/for_sale/'+city
    else:
        url = 'https://www.zillow.com/homes/for_sale/'+city+str(page)+'_p/'
    with requests.Session() as s:
        r = s.get(url, headers=req_headers)
        data = json.loads(re.search(r'!--(\{"queryState".*?)-->', r.text).group(1))
    urls.append(url)
    data_list.append(data)
```
This code chunk was very unique to what I was doing, and super efficient as gathering many links and pages. 
I also included an important column in my opinion which was price/sq ft. This really allows any sort of reader to see what the price/sq ft is and see if it's overpriced.
```python
df['price per sq ft'] = df['unformattedPrice'] / df['area']
```

Lastly, I wanted to make sure I had a large dataset from compiled days. I decided to download each pull into a specific csv file for any future needs.
```python
## READ TO CSV FILE

compression_opts = dict(method='zip',
                        archive_name='slchomes_10_19.csv')  
homes.to_csv('slchomes_10_19.zip', index=False,
          compression=compression_opts) 
```

### Conclusion:
Go and give this code a whirl. Change the city to where you are located and see what other cool columns of information you can create, and don't forget to give cool visuals to boost your data presentation. Comment with your results below :)

Here is a link to the code:
[GitHub Repo](https://github.com/zadriscoll/zillow.git)
