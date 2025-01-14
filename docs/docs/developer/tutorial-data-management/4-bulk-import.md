---
title: Bulk import
author: Paul van Genuchten
date: 2023-05-09
---

## Introduction

This paragraph describes approaches to import metadata from existing repositories. Including an option to import metadata from a spreadsheet.

## Bulk import from spreadsheet

Many metadata initiatives tend to start from a spreadsheet. Each of the columns representa a metadata property and the rows are the individual records describing a resource. Spreadsheets have proven to be an effective medium to populate a catalogue with records. To facilitate this use case the GeoDataCrawler software provides an import spreadsheet method. The spreadsheet is parsed and a [MCF](./2-describing-resources.md) document is generated for every row.

Since every metadata initiative tends to have dedicated columns. A templating approach is used to convert from row to MCF. A default template is available, matching a default spreadsheet layout. If your spreadsheet layout is different, you need to adjust the template accordingly. 

- For this exercise we'll use the [LSC-hubs spreadsheet](https://git.wur.nl/isric/lsc-hubs/kenya-hub/-/blob/kenya/portals/KE/LSC/data.csv) in combination with the [LSC-hubs template](https://git.wur.nl/isric/lsc-hubs/kenya-hub/-/blob/kenya/portals/KE/LSC/data.j2). Notice that the template share the filename, but with extension `.j2`. Download both files to a new folder, called `csv`, in your working directory.
- From your shell environment run this command:

```
crawl-metadata --mode=import-csv --dir="./csv"
```

- If there are errors, check the paths and consider to open the CSV in Google Sheets and export it again or open it in a text editor to look for special cases. A known issue with this approach is that the crawler tool can not manage `newline` characters in text fields.
- Open one of the generated MCF files to evaluate its content.
- A common spreadsheet tool is [Microsoft Excel](https://www.microsoft.com/en-gb/microsoft-365/excel). If you open and export a spreadsheet from Excel, the CSV will use the ';' character as column separator. Use the --sep=';' parameter to indicate GeoDataCrawler to use this separator.

```
crawl-metadata --mode=import-csv --dir="./csv" --sep=';'
```

## Bulk import from an online location

Many resources are already described elsewhere which may be of interest to add to our catalogue. For this use case some options exist to import remote metadata. In many cases you want to import a selected subset of a remote catalogue. In this scenario we prepare a csv with the identifiers of the records to be imported. These identifiers are used to create a very minimal MCF file. GeoDataCrawler subsequently extends the local record with remote content. Currently supported are metadata from CSW, WMS and DOI.

- For this exercise download a [spreadsheet with a subset of the isric.org data repository](https://git.wur.nl/isric/lsc-hubs/kenya-hub/-/raw/kenya/portals/Global/data.isric.org/datasets.csv?inline=false) and the [relevant template](https://git.wur.nl/isric/lsc-hubs/kenya-hub/-/raw/kenya/portals/Global/data.isric.org/datasets.j2?inline=false) to a folder `isric`.
- Import the CSV

```
crawl-metadata --mode=import-csv --dir="./isric" --sep=';'
```

- The synchronisation with remote content is implemented in GeoDataCrawler as part of the update metadata method. The update process will evaluate the dataseturi-property. If the uri refers to remote content in a supported format, the MCF will be updated against that content (remote takes preference).

```
crawl-metadata --mode=update --dir="./isric"
```

- Notice the changes before and after running the script. If needed, you can remove all the MCF files and run import-csv again to restore the originals.

