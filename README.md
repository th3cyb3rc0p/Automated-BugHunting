# Automated-BugHunting

## Recon

1. Get a target *.someurl.com
2. Start to enum with tools. 

- Subdomains
  - Sublister
  - Amass
  ```bash
  amass enum -active -brute -d web.site.com -o sitesout.txt 
  ```
- urls (results from subdomains) 
  - Sort results
  ```bash
  cat sitesout.txt | sort -u | uniq -u | httpx -silent > $file-alive.txt
  ```
- Parameters
  - hakrawler sort
  ```bash
  cat $file-alive.txt | hakrawler -depth 5 -plain -wayback | anew $hakrawler.txt &> /dev/null
  ```
  - Paramspider
  ```bash
  python3 ~/tools/ParamSpider/paramspider.py --domain "site.com" --exclude woff,css,js,png,svg,jpg --level high --quiet --output paramspider.txt &> /dev/null
  ```
     Sort
     
  ```bash
  cat paramspiderout.txt hakrawler.out| sort -u | uniq -u > params_outfinal.txt
  ```
  
 - gf
   - sort
  ```bash
    cat params_outfinal.txt | gf xss | sed 's/=.*/=/' | sed 's/URL: //' > xss.txt
    cat params_outfinal.txt | gf ssrf > ssrf.txt
    cat params_outfinal.txt | gf ssti > ssti.txt
    cat params_outfinal.txt | gf redirect > redirect.txt
    cat params_outfinal.txt | gf sqli > sqli.txt
    cat params_outfinal.txt | gf lfi > lfi.txt
    cat params_outfinal.txt | gf rce > rce.txt
  ```
