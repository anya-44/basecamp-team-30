# Data Cleaning Log — London Road Speed Limits

## Why we chose this dataset

We need the locations of all primary schools in London to identify the surrounding road environments and the rates of child pedestrian collisions nearby. 

- **Reliable:** Department for Education (DfE), Get Information about Schools (GIAS). This is the official DfE database of educational establishments in England, providing publicly available school information and establishment records. 
- **Suitable:** provides information about individual schools, including their names, addresses, postcodes, establishment types, phases and operational status. This allows us to identify primary schools and locate them geographically.
- **Columns used:** "URN",
    "EstablishmentName",
    "Easting",
    "Northing",
    "DistrictAdministrative (code)",
    "LSOA (code)",
    "TypeOfEstablishment (name)",
    "StatutoryLowAge",
    "StatutoryHighAge".
- **Area:** Greater London. Schools are filtered using their geographical location and London local authority boundaries to identify primary schools within the study area.
- **Feasible:** freely downloadable from the Department for Education's Get Information about Schools website. The download facility allows users to select specific data fields and export establishment records for analysis. 
- **Gaps:** the dataset provides information about schools rather than individual pupils or pedestrian activity. School locations represent establishment addresses, which may not precisely correspond to pupil entrances. The database is updated regularly, so downloaded records represent a snapshot of the information available at the time of extraction. It does not provide information about surrounding road conditions or pedestrian collisions; these must be obtained from separate datasets.
- **Licence:** publicly available data, subject to the licensing and attribution conditions stated by the Department for Education. Check the relevant download's licence before reuse. 


## Cleaning steps

|  | Action | Reason | Rows affected |
|---|---|---|---|
| 1 | Counted: Currently open primary schools = 27159; Opened after start of 2021 = 3723; Missing opening dates = 30039 | There are a lot of primary schools with missing opening dates in the unfiltered dataset so we must filter dates carefully | 0 (just a count) |
| 2 | df.info() | Check the types of columns and to check for blanks | 0 | 
| 3 | df.describe() | Check for impossible numbers | 0 | 
| 4 | df.duplicated().sum() | Check for repeated rows | 0 |
| 5 | Removed schools outside London and made note of schools with bad location data | Schools removed: 46795; Schools with missing or zero coordinates: 22; Schools outside London coordinate bounds: 3 | 46795 |
| 5.1 (TO DO) | Deal with the 25 MISC schools by manually looking up addresses and finding the corresponding postcode -> free postcodes.io API -> latitude/longitude -> Easting and Northing -> update the original dataframe | N/A | N/A | 
| 6 | Removed all non-primary schools | Included removal of "middle-deemed primary" and "all-through" schools because they introduce greater presence of older children - we ran counts and there were only 86 of these in London, and therefore there would still be plenty of data remaining if they were removed | 2781 |
| 7 | Applied temporal filtering to construct a balanced sample of schools that were operational across the full five-year observation window, in case this influenced accident levels in the area. Keep schools that: - Opened before or on 1 January 2021, OR have a missing opening date; - Have no closing date, OR closed after 31 December 2025; - Exclude schools that opened after 1 January 2021; - Exclude schools that closed during 2021–2025 |  Considered just removing all schools with unknown dates but we found that this removed far too many schools from the data. We are aware that a  missing opening date does not prove that a school existed in 2021, but it seems likely to us that the school is simply old enough that it predates the records. This leaves us with 1672 schools; 1006 of which have no opening date.| 1109 |
| 8 | Checked and logged special & independent schools inside our dataset | In case this provides interesting information later on. | 0 |
| 9 | Kept only the required columns, and ran df = df.reset_index(drop=True) | Minimal table size - just to tidy it up. | 0 |

1672 schools remain.


