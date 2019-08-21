Getting started with edgarapi
**********************************

Install
=======

Install via::

    pip install edgarapi

Usage
=========


Download Filings of a Particular Type for All Companies
-------------------------------------------------------------

This is an example::

    import os
    import edgar

    root_folder = 'filings'
    filing_type = '10-K'
    sub_document_type = 'EX-21.01'

    ed = edgar.Edgar()

    for name, cik in ed.all_companies_dict.items():

        # Get company filing objects
        company = edgar.Company(name, cik)
        tree = company.get_all_filings(filing_type = filing_type)
        filings = edgar.get_filings(tree, no_of_documents=100)

        # Set folder for this company and make it
        company_folder = os.path.join(root_folder, cik)
        if not os.path.exists(company_folder):
            os.makedirs(company_folder)

        # Output company docs
        for filing in filings:
            doc_path = os.path.join(company_folder, filing.period_of_report + '.html')
            with open(doc_path, 'w') as f:
                f.write(filing.sub_filing(sub_document_type, as_html=True))

