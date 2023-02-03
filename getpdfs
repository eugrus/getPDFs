#!/bin/bash

if [ $# -eq 0 ]; then
  echo "This script will download all PDFs linked from a given web page. Please provide a web page's URL as an argument."
  echo "If cookies needed to access the web page, export them into cookies.txt in the same folder."; echo ""
  echo "https://github.com/eugrus/getPDFs"; echo ""
  echo "The following terms apply: https://creativecommons.org/licenses/by-nc-sa/3.0/de/deed.en"; echo ""
  exit 1
fi

url=$1

# Getting the web page
html=$(curl -s -A "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.149 Safari/537.36" -J -b cookies.txt $url)

# Extracting links to PDFs
pdfs=$(echo "$html" | grep -oE 'href="[^"]+\.pdf"' | sed 's/href="//g' | sed 's/"$//g')

# Getting rid of "index.php" in the URL if provided (important for relative links)
if [[ "$url" == *"index.php"* ]]; then
  url=${url%index.php*}
fi

# Downloading
for pdf in $pdfs; do
  # Checking if the PDF's URL is relative or absolute
  if [[ "$pdf" != *"http"* ]]; then
    # If relative, converting to absolute URL
    pdf="$url/$pdf"
  fi
  echo $pdf
  curl -O $pdf
done

echo "Downloaded $(echo "$pdfs" | wc -w) PDFs."
