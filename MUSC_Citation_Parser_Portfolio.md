[Back to Portfolio](./)

MUSC PDF Citation Parser – Research Paper Analysis
=================================================

- **Class:** CSCI 495 (Fall 2025)
- **Grade:** *[Enter project/course grade]*
- **Language(s):** Python, HTML (Jinja2 templates), CSS; SQLite/SQL for data storage. The interface also uses HTMX and Hyperscript.
- **Source Code Repository:** [CSCI 495 Green Team Citation Parser](https://github.com/csu-cs/csci495-Fall25-greenTeam-citationParser)  
  *This is the GitHub URL supplied in the project's `pyproject.toml`. If your portfolio must link to a different **private** repository, replace this link and add your preferred access-request instructions.*

## Project description

The MUSC PDF Citation Parser is a team-developed research application designed to help users analyze citations in academic PDF documents. The project, identified in its package metadata as a PDF citation processor for the Medical University of South Carolina (MUSC), converts research papers into searchable text and identifies where numbered references are used throughout a document. Instead of manually locating every occurrence of a citation, users can browse the extracted references and examine the sentences and paragraphs in which those references appear.

The application combines a Python PDF-processing pipeline with a Flask web interface and a SQLite database. Uploaded PDF files are converted into Markdown-formatted text using PyMuPDF4LLM. The parser normalizes extracted characters, identifies entries in a numbered references section, separates the document into paragraphs and sentences, and detects bracketed citations such as `[3]`, `[2, 5]`, or `[4-7]`. It then associates the citations it recognizes with their corresponding bibliography entries and stores the results for later review.

The website provides pages for uploading papers, viewing previously processed documents, examining citations alongside their original context, searching reference entries, and searching the stored text of papers. Celery and a Redis-compatible Valkey service support background PDF-processing tasks so the web interface can show the status of an upload. The repository also contains experimental language-model summarization functions; however, the supplied version uses mock AI settings and does not enable completed AI-generated citation explanations.

## How to compile and run the program

This project is written in Python, so no separate compilation step is required. From the project's root directory, run:

```bash
make run
```

Once the server starts, open `http://127.0.0.1:5000/` in a web browser.

## UI Design and Program Operation

The application uses a browser-based interface rather than a traditional desktop or console menu. Its templates use Bootstrap styling with HTMX-driven interactions, while Flask provides the pages and database queries.

**1. Home page and paper list.** The home page displays a *Current Papers* section and a search field for finding previously uploaded papers by title. Each stored paper has a filename, upload date, and processing status. The listing also provides actions to view a paper's details, edit its title, or delete its record.

**2. PDF upload and processing.** The *Parse a PDF* page accepts a PDF file and a paper title. After submission, Flask saves the upload and queues the `process_pdf` task through Celery. The page includes an upload-progress indicator, followed by a waiting screen while processing is underway. Once the task finishes, its result page is designed to display a table of references and the number of citations found for each.

**3. Citation extraction and storage.** The parser converts a PDF into Markdown-like text, normalizes selected unusual characters, and scans for a numbered *References* section. It then examines paragraphs and sentences for bracketed numeric citations. Single citations, comma-separated values, and numeric ranges can be recognized. Related paper, paragraph, reference, and citation records are stored using Flask-SQLAlchemy and SQLite. The stored citation record retains the sentence and surrounding paragraph, making it possible to review a reference in context.

**4. Paper details.** The *Paper with References* page displays extracted paragraphs beside a table of the references cited in each paragraph. The citation table shows the bibliography entry and the sentence containing its citation. Users can follow a reference link to see other occurrences of the same source.

**5. Reference research and text search.** The *View Research* page lists stored bibliography entries and links them to papers in which they were cited. A search field narrows the reference list. A separate *Full Text Search* page searches the stored paragraphs and links matching text back to its paper, allowing users to find discussions of a topic without opening each original PDF.

### Screenshots and numbered captions

The images below are **placeholders for genuine screenshots** of the running application; no verified application screenshots were included in the ZIP. Save your screenshots in an `images` folder alongside this Markdown page, then replace the commented placeholders with image links in the format `![Description](images/Filename.png)`.

<!-- Screenshot needed: ![Current papers page](images/CitationParserHome.png) -->
**Fig. 1.** Main page showing the list of uploaded research papers and the paper-title search field. *[Screenshot needed]*

<!-- Screenshot needed: ![PDF upload form](images/CitationParserUpload.png) -->
**Fig. 2.** Parse a PDF page where the user chooses a research document and enters its title. *[Screenshot needed]*

<!-- Screenshot needed: ![PDF parsing results](images/CitationParserResults.png) -->
**Fig. 3.** Processing results showing extracted bibliography entries and their citation counts. *[Screenshot needed]*

<!-- Screenshot needed: ![Paper details](images/CitationParserPaperDetails.png) -->
**Fig. 4.** Paper details page displaying document paragraphs alongside their associated citations and reference entries. *[Screenshot needed]*

<!-- Screenshot needed: ![Research references](images/CitationParserResearch.png) -->
**Fig. 5.** Research page listing bibliography entries and linking them to papers that cite those sources. *[Screenshot needed]*

<!-- Screenshot needed: ![Reference occurrences](images/CitationParserReference.png) -->
**Fig. 6.** Individual reference page identifying the papers and sentences in which a selected source was cited. *[Screenshot needed]*

<!-- Screenshot needed: ![Full text search](images/CitationParserSearch.png) -->
**Fig. 7.** Full Text Search page displaying matching paragraphs and links to their original paper records. *[Screenshot needed]*

For example, after capturing the upload form, replace its commented placeholder with:

```markdown
![PDF upload form](images/CitationParserUpload.png)  
Fig. 2. Parse a PDF page where the user chooses a research document and enters its title.
```

## Additional Considerations

This project illustrates how a larger research workflow can be divided into specialized components. The `pdf_parse` package handles text conversion, reference identification, and citation matching; the `pdf_database` package defines persistent records and relationships; and the `pdf_frontend` package provides the user-facing pages, routes, and background task integration. These components allow the application to present extracted research data in ways that are easier to browse and search than a collection of individual PDF files.

An important technical challenge is that academic PDFs do not always preserve a clean reading order or consistent formatting when converted into text. The project approaches this by transforming PDFs to Markdown, normalizing selected characters, detecting reference headings, and using regular expressions to recognize bracketed numeric citations. These methods work best with papers following the expected numbered-citation format; unusual layouts, author–date citations, inconsistent reference sections, or conversion errors may require further refinement.

The project also demonstrates collaboration around a multi-component Python application and the use of asynchronous processing to separate potentially time-consuming PDF analysis from the web interface. Its design anticipates additional AI-assisted research features, but the provided code retains mock summarization functions and commented-out calls to the language model. The current implementation is best described as a **PDF citation extraction, indexing, and search application**, with AI summarization remaining an area for future development.

[Back to Portfolio](./)
