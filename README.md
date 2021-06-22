Post-correction of OCR results
==============================
The proposed task concerns post-correcting OCR results of Polish-language books, which were published in the years 1791-1998.

Motivation
----------
Although Optical Character Recognition methods have a long history of development, their performance is still limited, especially in the case of low-quality source material, historical texts and non-standard document layouts. The extent to which such methods may be improved based on methods relying on image analysis is also limited, as some parts of source documents might simply be unrecognizable because of visual defects or distortions in data. In another scenario, the quantity of the training data might not allow to train a representative model for the specific language variant used in a set of documents. For example, in the comparison of recognition results of two popular OCR engines published by the IMPACT project (https://www.digitisation.eu/fileadmin/Tool_Training_Materials/Abbyy/PSNC_Tesseract-FineReader-report.pdf) the recognition rate for historical documents (published before 1850, in Polish) was 80% on character level and ca. 60% on word level. Such accuracy levels are far from satisfactory in most applications as they result in error propagation in further stages of text processing.

On the other hand, we may observe considerable progress in Natural Language Processing methods, especially in the area of language modeling approaches based on deep neural networks. Models, such as BERT and its variants, prove to increase the accuracy of many NLP-related tasks, such as named entity recognition, fake news detection, or question answering. The motivation of this task is thus to evaluate the possible improvement in OCR results by using modern NLP approaches to language error correction. In a similar task (https://sites.google.com/view/icdar2019-postcorrectionocr), the improvement of OCR results reported by the best overall performer submitted for the challenge ranged from 6% for Spanish to 24% for German (https://drive.google.com/file/d/15mxNO-M9PiXBnffi7MOa8wUw33nj1xBp/view). For some specific languages and submitted methods the results were even better, such as 44% for Finnish and 26% for French.

Dataset
-------
The dataset contains Polish language books, published in the period of 1791-1998, which are currently freely licenced. The source material has been adapted from the Wikisource project by collecting original scans and their manual transcriptions from Wikisource and using Tesseract OCR engine to generate (potentially erroneous) text versions of the original scans. The originial page number, book ID and book publishing date (not available for some books) is included in the dataset and may be included in the trained model. Only pages with at least 150 characters are included in order to filter out front pages, covers, titles etc. which are not the target of this task.

For the purposes of the challenge we provide a training dataset, containing reference transcriptions, the material produced by the OCR engine. We also provide a development dataset, which may be used to analyze the performance of a particular model in detail. For both of these datasets an alignment between gold-standard data and OCR-results is also provided. The test-A dataset, for which gold-standard data is not provided will be used to generate the live leaderboard of the challenge. The final evaluation will be performed on the test-B dataset, which will be revealed ca. one week before the competition end.

The whole set contains 979 books with a total number of almost 69 000 pages. Books’ years of publication range from 1791 to 1998, with the majority of them coming from the years 1870-1940. The training set contains roughly 46 000 pages, while the development set contains 5500 pages.

The data has been published in the following repository: https://github.com/poleval/2021-ocr-correction

Metric
------

We will use the WER metric to rank the submissions.

Input
-----

Each text is given as a separate line with 4 items separated by TABs:

1. Document ID
2. Page number
3. Year (sometimes unspecified)
4. Text to be corrected (output of an OCR system)

### Escaping special characters

The following escape sequences are used for the OCR-ed text:

* `\n` — end of line,
* `\\` — literal backslash.

### Sample Python code for how to parse input files

```
# for xz decompression
import lzma

with lzma.open('dev-0/in.tsv.xz') as fh:
    for line in fh:
        line = line.rstrip('\n')
        doc_id, page_no, year, in_text = line.split('\t')
        # unescape end-of-lines and backslashes
        in_text = in_text.replace('\\n', '\n').replace('\\\\', '\\')
        # do someting with in_text
```

Output
------

Output is just the text corrected.

Remember to escape end-of-lines and backslashes.
