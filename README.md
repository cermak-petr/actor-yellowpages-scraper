### Yellow Pages Scraper
 
Yellow Pages Scraper is an [Apify actor](https://apify.com/actors) for scraping information from Yellow Pages listings. It allows you to search records based on a combination of search term and location or a list of URLs. It is build on top of [Apify SDK](https://sdk.apify.com/) and you can run it both on [Apify platform](https://my.apify.com) and locally.  

- [Input](#input)
- [Output](#output)
- [Compute units consumption](#compute-units-consumption)
- [Extend output function](#extend-output-function)

### Input

| Field | Type | Description | Default value
| ----- | ---- | ----------- | -------------|
| search | string | Query string to be searched on the site | `"Dentist"` |
| location | string | Location string to search the records in | `"Los Angeles"` |
| startUrls | array | List of [Request](https://sdk.apify.com/docs/api/request#docsNav) objects that will be deeply crawled. The URL can be any Yellowpages.com record list page | none |
| maxItems | number | Maximum number of pages that will be scraped | `200` |
| extendOutputFunction | string | Function that takes a Cheerio object and a Cheerio representation of the record element ($, record) as arguments and returns data that will be merged with the default output. More information in [Extend output function](#extend-output-function) | `($, record) => { return {}; }` |
| proxyConfiguration | object | Proxy settings of the run. If you have access to Apify proxy, you can set `{ "useApifyProxy": true" }` to enable proxy usage | `{ "useApifyProxy": false }`|  

Either the `search` and `location` attributes or the `startUrls` atrribute has to be set.

### Output

Output is stored in a dataset. Each item is information about a record.
```
{
  "isAd": true,
  "url": "https://www.yellowpages.com/compton-ca/mip/golden-state-dental-group-18768214?lid=1001760866489",
  "name": "Golden State Dental Group",
  "address": "1601 N Long Beach Blvd, Compton, CA 90221",
  "phone": "(310) 507-7718",
  "rating": 4,
  "ratingCount": 6,
  "infoSnippet": "*Please contact us for more information",
  "image": "https://i4.ypcdn.com/blob/00a40d49e577606be9d82ced5404696022a7e2a0",
  "categories": [
    "Dentists",
    "Implant Dentistry",
    "Pediatric Dentistry",
    "Periodontists",
    "Cosmetic Dentistry"
  ]
}
```
Please note that not all of the attributes will be present with all the results.

### Compute units consumption
Keep in mind that it is much more efficient to run one longer scrape (at least one minute) than more shorter ones because of the startup time.

The average consumption is about **0.04 Compute unit per 2000 results** scraped.

### Extend output function

You can use this function to update the default output of this actor. This function takes a Cheerio object and a Cheerio representation of the record element ($, record) as arguments, so you can choose which other attributes you would like to add. The output from this function will get merged with the default output.

The return value of this function has to be an object!

You can return fields to achieve 3 different things:
- Add a new field - Return object with a field that is not in the default output
- Change a field - Return an existing field with a new value
- Remove a field - Return an existing field with a value `undefined`

```
($, record) => {
    return {
        directionsLink: record.find('.directions').attr('href'),
        rating: 5,
        phone: undefined,
    }
}
```
This example will add a new field `directionsLink`, change the `rating` field and remove the `phone` field

### Epilogue
Thank you for trying my actor. You can send any feedback you have to my email `cermak.petr6@gmail.com`.  
If you find any bug, please create an issue on the [Github page](https://github.com/cermak-petr/actor-yellowpages-scraper).
