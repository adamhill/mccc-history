This is an archive of the Mid Cities Commodore Computer Club's newsletters and some anniversary pics.
### We have
- An archive of the newletters in PDF format
- A collection of photos from the 20th Reunion
- Various photos from Jim Butterfield speaking at Metcom(?)

I have:
- Converted all the PDF's to markdown using [Docling](https://github.com/docling-project/docling) with:
```bash
uvx docling *.pdf --from=pdf --to=md --num-threads 16 --image-export-mode referenced --verbose
```
This converts the PDF to MD, using Doclings defaults, extracts images and links them to the MD
It does a pretty good job but has a little issue with dot matrix printers and the format of the early newsletters (84-90) But nothing that a little LLM can't fixup.

- All the PDF's are about as small as they can get (monochrome - for the early ones, CCCIT (Fax) compression)
- Some minor cleanup for the 1987-1990 time frame for formatting end matter in the newsletters - BOD, Officers, Meetings, BBS's. Mainly as an exploration of what and how to clean things up
- Some cleaup for BASIC programs in the early '80 that were published. Mainly LLM + a screen grab of the section.
- All the later ones, post 2000-ish are pretty clean already.
- If anyone wants to submit PR's to clean up further, they are happily accepted
  - If we need to make a checklist per year if other people want to help out we can do that too.

### Checklist
- [] 1983
- [] 1984
- [] 1985
- [] 1986
- [] 1987
- [] 1988
- [] 1989
- [] 1990
