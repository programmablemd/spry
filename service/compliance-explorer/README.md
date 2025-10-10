---
siteName: compliance-explorer
sqlpage-conf:
  database_url: "sqlite://sqlpage.db?mode=rwc"  
  web_root: "./"
  allow_exec: true  
  port: 9221
---

The compliance explorer covers a wide range of standards and guidelines across different areas of cybersecurity and data protection. They include industry-specific standards, privacy regulations, and cybersecurity frameworks. Complying with these frameworks supports a strong cybersecurity stance and alignment with data protection laws.

The content in this folder is authored as a markdown-driven SQLPage page (`README.md`) and the site is configured to use a local SQLite database (`sqlpage.db`).

## Layout

```sql LAYOUT
-- global LAYOUT (defaults to **/*)



SELECT 'shell' AS component,
       'Compliance Explorer' AS title,
       NULL AS icon,
       'https://www.surveilr.com/assets/brand/content-assembler.ico' AS favicon,
       'https://www.surveilr.com/assets/brand/compliance-explorer.png' AS image,
       'fluid' AS layout,
       true AS fixed_top_menu,
       'index.sql' AS link,
       '{"link":"./index.sql","title":"Home"}' AS menu_item;

SET resource_json = sqlpage.read_file_as_text('spry.d/auto/resource/${ctx.path}.auto.json');
SET page_title  = json_extract($resource_json, '$.route.caption');

```

## Files

- `README.md` — the page source (markdown + SQL/JSON/JS/etc..) that defines the site content and cards.
- `ingest/` — CSV files and supporting data to import into the local `sqlpage.db` (not all files listed here).

The below SQL code first sets a variable (resource_json) from the JSON file and extracts the page caption. Then it adds an introductory paragraph explaining the purpose of the compliance explorer. Finally, it renders a responsive card layout (2 columns) listing key cybersecurity and data protection standards — such as CMMC, AICPA, HiTRUST, ISO 27001, HIPAA, and THSA — each with structured markdown descriptions (geography, source, version, and review date).

```sql index.sql { route: { caption: "Compliance Explorer" } }


SELECT
  'text' AS component,
   $page_title AS title;

SELECT
  'The compliance explorer covers a wide range of standards and guidelines across different areas of cybersecurity and data protection. They include industry-specific standards, privacy regulations, and cybersecurity frameworks. Complying with these frameworks supports a strong cybersecurity stance and alignment with data protection laws.' AS contents;

SELECT
  'card' AS component,
  '' AS title,
  2 AS columns;

SELECT
  'CMMC' AS title,
  '**Geography**: US 

  **Source**: Department of Defense (DoD) 

  **Version**: 2.0 

  **Published/Last Reviewed Date/Year**: 2021-11-04 00:00:00+00' AS description_md,      
  sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/ce/regime/cmmc.sql' as link
UNION
SELECT
  'AICPA' AS title,
  '**Geography**: US 

  **Source**: American Institute of Certified Public Accountants (AICPA) 

  **Version**: N/A 

  **Published/Last Reviewed Date/Year**: 2023-10-01 00:00:00+00' AS description_md,
  sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/ce/regime/aicpa.sql' as link
UNION
SELECT
  'HiTRUST e1 Assessment' AS title,
  '**Geography**: US 

  **Source**: HITRUST Alliance 

  **HITRUST Essentials, 1-Year (e1) Assessment** 

  **Version**: e1 

  **Published/Last Reviewed Date/Year**: 2021-09-13 00:00:00+00' AS description_md,      
  sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/ce/regime/hitrust.sql' as link
UNION
SELECT
  'ISO 27001:2022' AS title,
  '**Geography**: International 

  **Source**: International Organization for Standardization (ISO) 

  **Version**: 2022 

  **Published/Last Reviewed Date/Year**: 2022-10-25 00:00:00+00' AS description_md,      
  sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/ce/regime/iso-27001.sql' as link
UNION
SELECT
  'HIPAA' AS title,
  '**Geography**: US 

  **Source**: Federal 

  **Health Insurance Portability and Accountability Act (HIPAA)** 

  **Version**: N/A 

  **Published/Last Reviewed Date/Year**: 2024-01-06 00:00:00+00' AS description_md,
  sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/ce/regime/hipaa_security_rule.sql' AS link
UNION
SELECT
  'Together.Health Security Assessment (THSA)' AS title,
  '**Geography**: US 

  **Source**: Together.Health (health innovation collaborative) 

  **Together.Health Security Assessment (THSA)** 

  **Version**: v2019.1 

  **Published/Last Reviewed Date/Year**: 2019-10-26 00:00:00+00' AS description_md,      
  sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/ce/regime/thsa.sql' AS link;
```
AICPA page

```sql ce/regime/aicpa.sql { route: { caption: "AICPA" } }
-- Display breadcrumb
SELECT
  'breadcrumb' AS component;
SELECT
  'Home' AS title,
  sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/' AS link;
SELECT
  'AICPA' AS title,
  sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/ce/regime/aicpa.sql' AS link;
 
SELECT 'title' AS component, (SELECT COALESCE(title, caption)
    FROM sqlpage_aide_navigation
   WHERE namespace = 'prime' AND path = 'ce/regime/aicpa.sql/index.sql') as contents;
    ;
 
SELECT
  'text' AS component,
  $page_title AS title;
 
SELECT
  'The American Institute of Certified Public Accountants (AICPA) is the national professional organization for Certified Public Accountants (CPAs) in the United States. Established in 1887, the AICPA sets ethical standards for the profession and U.S. auditing standards for private companies, nonprofit organizations, federal, state, and local governments. It also develops and grades the Uniform CPA Examination and offers specialty credentials for CPAs who concentrate on personal financial planning; forensic accounting; business valuation; and information technology.' AS contents;
 
-- Cards for SOC 2 Type I & Type II
SELECT
  'card' AS component,
    2 AS columns;
 
SELECT
  'SOC 2 Type I' AS title,
  'Report on Controls as a Service Organization. Relevant to Security, Availability, Processing Integrity, Confidentiality, or Privacy.' AS description,
  sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/ce/regime/soc2_type1.sql' AS link
UNION ALL
SELECT
  'SOC 2 Type II' AS title,
  'SOC 2 Type II reports provide lists of Internal controls that are audited by an Independent third-party to show how well those controls are implemented and operating.' AS description,
  sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/ce/regime/soc2_type2.sql' AS link;

```
SOC 2 Type I Controls page

```sql ce/regime/soc2_type1.sql { route: { caption: "SOC 2 Type I Controls" } }
--- Display breadcrumb
SELECT
  'breadcrumb' AS component;
SELECT
  'Home' AS title,
  sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/' AS link;
SELECT
  'AICPA' AS title,
  sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/ce/regime/aicpa.sql' AS link;
SELECT
  'SOC 2 Type I' AS title,
  sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/ce/regime/soc2_type1.sql' AS link;
 
SELECT 'title' AS component, (SELECT COALESCE(title, caption)
    FROM sqlpage_aide_navigation
   WHERE namespace = 'prime' AND path = 'ce/regime/soc2_type1.sql/index.sql') as contents;
    ;
 
SELECT
  'text' AS component,
  $page_title AS title;
 
SELECT
    'The SOC 2 controls are based on the AICPA Trust Services Criteria, focusing on security, availability, processing integrity, confidentiality, and privacy.' AS contents;
 
SELECT
  'table' AS component,
  "Control Code" AS markdown,
  TRUE AS sort,
  TRUE AS search;
 
-- Pagination Controls (Top)
SET total_rows = (SELECT COUNT(*) FROM compliance_regime_control_soc2 );
SET limit = COALESCE($limit, 50);
SET offset = COALESCE($offset, 0);
SET total_pages = ($total_rows + $limit - 1) / $limit;
SET current_page = ($offset / $limit) + 1;
 
SELECT
  '[' || control_id || '](' ||
    sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/ce/regime/soc2_detail.sql?type=soc2-type1&id=' || control_id || ')' AS "Control Code",
    control_name AS "Control Name",
    common_criteria AS "Common Criteria",
    criteria_type AS "Criteria Type",
    control_question AS "Control Question"
FROM compliance_regime_control_soc2
LIMIT $limit OFFSET $offset;
 
-- Pagination Controls (Bottom)
SELECT 'text' AS component,
    (SELECT CASE WHEN CAST($current_page AS INTEGER) > 1 THEN '[Previous](?limit=' || $limit || '&offset=' || ($offset - $limit) || ')' ELSE '' END)
    || ' '
    || '(Page ' || $current_page || ' of ' || $total_pages || ") "
    || (SELECT CASE WHEN CAST($current_page AS INTEGER) < CAST($total_pages AS INTEGER) THEN '[Next](?limit=' || $limit || '&offset=' || ($offset + $limit) || ')' ELSE '' END)
    AS contents_md
;
             
```

SOC 2 Type II Controls page

```sql ce/regime/soc2_type2.sql { route: { caption: "SOC 2 Type II Controls" } }

--- Display breadcrumb
SELECT
  'breadcrumb' AS component;
SELECT
  'Home' AS title,
  sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/' AS link;
SELECT
  'AICPA' AS title,
  sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/ce/regime/aicpa.sql' AS link;
SELECT
  'SOC 2 Type II' AS title,
  sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/ce/regime/soc2_type2.sql' AS link;
 
--- Display page title
SELECT
  'title' AS component,
  $page_title AS contents;
 
--- Display description
SELECT
  'text' AS component,
  'SOC 2 Type II reports evaluate not just the design, but also the operating effectiveness of controls over a defined review period.' AS contents;
 
--- Table
SELECT
  'table' AS component,
  "Control Code" AS markdown,
  TRUE AS sort,
  TRUE AS search;
 
-- Pagination Controls (Top)
SET total_rows = (SELECT COUNT(*) FROM aicpa_soc2_type2_controls );
SET limit = COALESCE($limit, 50);
SET offset = COALESCE($offset, 0);
SET total_pages = ($total_rows + $limit - 1) / $limit;
SET current_page = ($offset / $limit) + 1;
 
SELECT
  '[' || control_id || '](' ||
    sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/ce/regime/soc2_detail.sql?type=soc2-type2&id=' || control_id || ')' AS "Control Code",
  fii_id AS "FII ID",
  common_criteria AS "Common Criteria",
  criteria_type AS "Criteria Type",
  control_name AS "Control Name",
  control_question AS "Control Question"
FROM aicpa_soc2_type2_controls
LIMIT $limit OFFSET $offset;
 
-- Pagination Controls (Bottom)
SELECT 'text' AS component,
    (SELECT CASE WHEN CAST($current_page AS INTEGER) > 1 THEN '[Previous](?limit=' || $limit || '&offset=' || ($offset - $limit) || ')' ELSE '' END)
    || ' '
    || '(Page ' || $current_page || ' of ' || $total_pages || ") "
    || (SELECT CASE WHEN CAST($current_page AS INTEGER) < CAST($total_pages AS INTEGER) THEN '[Next](?limit=' || $limit || '&offset=' || ($offset + $limit) || ')' ELSE '' END)
    AS contents_md
;      

```

SOC 2 Type II Control Detail page

```sql ce/regime/soc2_detail.sql { route: { caption: "AICPA" } }


    -- Breadcrumbs
    SELECT 'breadcrumb' AS component;
    SELECT 'Home' AS title, sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/' AS link;    
    SELECT 'AICPA' AS title, sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/ce/regime/aicpa.sql' AS link;
 
    -- SOC 2 Type breadcrumb
    SELECT
      CASE
        WHEN $type = 'soc2-type1' THEN 'SOC 2 Type I'
        WHEN $type = 'soc2-type2' THEN 'SOC 2 Type II'
        ELSE 'SOC 2'
      END AS title,
      CASE
        WHEN $type = 'soc2-type1' THEN sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/ce/regime/soc2_type1.sql'
        WHEN $type = 'soc2-type2' THEN sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/ce/regime/soc2_type2.sql'
        ELSE sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/ce/regime/aicpa.sql'
      END AS link;
 
    -- Last breadcrumb (dynamic control_id, non-clickable)
    SELECT
      control_id AS title, '#' AS link
    FROM (
      SELECT control_id
      FROM compliance_regime_control_soc2
      WHERE $type = 'soc2-type1' AND control_id = $id::TEXT
      UNION ALL
      SELECT control_id
      FROM aicpa_soc2_type2_controls
      WHERE $type = 'soc2-type2' AND control_id = $id::TEXT
    ) t
    LIMIT 1;
 
    -- Card Header
    SELECT 'card' AS component,
           CASE
             WHEN $type = 'soc2-type1' THEN 'SOC 2 Type I Control Detail'
             WHEN $type = 'soc2-type2' THEN 'SOC 2 Type II Control Detail'
             ELSE 'SOC 2 Control Detail'
           END AS title,
           1 AS columns;
 
    -- Detail Section (aligned UNION)
    SELECT
      common_criteria AS title,
      '**Control Code:** ' || control_id || '  

' ||
      '**Control Name:** ' || control_name || '  

' ||
      (CASE WHEN $type = 'soc2-type2' THEN '**FII ID:** ' || COALESCE(fii_id,'') || '  

' ELSE '' END) ||
      '**Control Question:** ' || COALESCE(control_question,'') || '  

'
      AS description_md
    FROM (
      -- Type I controls (with SCF reference)
      SELECT control_id, control_name, fii_id, common_criteria, control_question
      FROM compliance_regime_control_soc2
      WHERE $type = 'soc2-type1' AND control_id = $id::TEXT
     
      UNION ALL
     
      -- Type II controls (no SCF reference → add NULL for column alignment)
      SELECT control_id, control_name, fii_id, common_criteria, control_question
      FROM aicpa_soc2_type2_controls
      WHERE $type = 'soc2-type2' AND control_id = $id::TEXT
    );
    -- TODO Placeholder Card
    SELECT
      'card' AS component,
      1 AS columns;
 
 
   -----accordion start
   SELECT 'html' AS component,
  '<details class="test-detail-outer-accordion" open>
    <summary class="test-detail-outer-summary">
      Policy Generator Prompt 
  <br>
  Create tailored policies directly for <b>Control Code: ' || $id || '</b> &mdash; <b>FII ID: ' || fii_id || '</b>.
  The "Policy Generator Prompt" lets you transform abstract requirements into actionable, 
  written policies. Simply provide the relevant control or framework element, and the prompt
  will guide you in producing a policy that aligns with best practices, regulatory standards, 
  and organizational needs. This makes policy creation faster, consistent, and accessible—even 
  for teams without dedicated compliance writers.
    </summary>
    <div class="test-detail-outer-content">' AS html
FROM (SELECT control_id, fii_id
    FROM compliance_regime_control_soc2
    WHERE $type = 'soc2-type1' AND control_id = $id::TEXT
    
    UNION ALL
    
    SELECT control_id, fii_id
    FROM aicpa_soc2_type2_controls
    WHERE $type = 'soc2-type2' AND control_id = $id::TEXT
)

     
    SELECT 'card' as component, 1 as columns;
    SELECT
      '
' || p.body_text AS description_md
      FROM ai_ctxe_complaince_prompt p
      WHERE p.control_id = $id AND p.documentType = 'Author Prompt' AND (
    ($type = 'soc2-type1' AND regime = 'SOC2-TypeI') OR
    ($type = 'soc2-type2' AND regime = 'SOC2-TypeII')
  );
      

    
    SELECT 'html' AS component,
      '</div></details>' AS html;

      --accordion for audit prompt

SELECT 'html' AS component,
  '<details class="test-detail-outer-accordion" open>
    <summary class="test-detail-outer-summary">
      Policy Audit Prompt 
      <br>
      Ensure your policies stay effective and compliant with the "Policy Audit Prompt". These prompts are designed to help users critically evaluate existing policies against standards, frameworks, and internal expectations. By running an audit prompt, you can identify gaps, inconsistencies, or outdated language, and quickly adjust policies to remain audit-ready and regulator-approved. This gives your team a reliable tool for continuous policy improvement and compliance assurance.
    </summary>
    <div class="test-detail-outer-content">' AS html
FROM (SELECT control_id, fii_id
    FROM compliance_regime_control_soc2
    WHERE $type = 'soc2-type1' AND control_id = $id::TEXT
    
    UNION ALL
    
    SELECT control_id, fii_id
    FROM aicpa_soc2_type2_controls
    WHERE $type = 'soc2-type2' AND control_id = $id::TEXT
)

    SELECT 'card' as component, 1 as columns;
    SELECT
      '
' || p.body_text AS description_md
      FROM ai_ctxe_complaince_prompt p
      WHERE p.control_id = $id AND p.documentType = 'Audit Prompt' AND (
    ($type = 'soc2-type1' AND regime = 'SOC2-TypeI') OR
    ($type = 'soc2-type2' AND regime = 'SOC2-TypeII')
  );
      
 SELECT 'html' AS component,
      '</div></details>' AS html;

      
SELECT 'html' AS component,
  '<details class="test-detail-outer-accordion" open>
    <summary class="test-detail-outer-summary">
      Generated Policies
      <br>
      The Generated Policies section showcases real examples of policies created using the "Policy Generator Prompt". These samples illustrate how high-level controls are translated into concrete, practical policy documents. Each generated policy highlights structure, clarity, and compliance alignment—making it easier for users to adapt and deploy them within their own organizations. Think of this as a living library of ready-to-use policy templates derived directly from controls.
    </summary>
    <div class="test-detail-outer-content">' AS html
FROM (SELECT control_id, fii_id
    FROM compliance_regime_control_soc2
    WHERE $type = 'soc2-type1' AND control_id = $id::TEXT
    
    UNION ALL
    
    SELECT control_id, fii_id
    FROM aicpa_soc2_type2_controls
    WHERE $type = 'soc2-type2' AND control_id = $id::TEXT
)

    SELECT 'card' as component, 1 as columns;
    SELECT
      '
' || p.body_text AS description_md
      FROM ai_ctxe_policy p
      WHERE p.control_id = $id AND (
    ($type = 'soc2-type1' AND regimeType = 'SOC2-TypeI') OR
    ($type = 'soc2-type2' AND regimeType = 'SOC2-TypeII')
  );
   SELECT 'html' AS component,
      '</div></details>' AS html;
      SELECT 'html' as component,
    '<style>
        tr.actualClass-passed td.State {
            color: green !important; /* Default to red */
        }
         tr.actualClass-failed td.State {
            color: red !important; /* Default to red */
        }
          tr.actualClass-passed td.Statealign-middle {
            color: green !important; /* Default to red */
        }
          tr.actualClass-failed td.Statealign-middle {
            color: red !important; /* Default to red */
        }
        
        .btn-list {
        display: flex;
        justify-content: flex-end;
        }
       h2.accordion-header button {
        font-weight: 700;
      }

      /* Test Detail Outer Accordion Styles */
      .test-detail-outer-accordion {
        border: 1px solid #ddd;
        border-radius: 8px;
        margin: 20px 0;
        overflow: hidden;
      }

      .test-detail-outer-summary {
        background-color: #f5f5f5;
        padding: 15px 20px;
        cursor: pointer;
        font-weight: 600;
        color: #333;
        border: none;
        outline: none;
        user-select: none;
        list-style: none;
        position: relative;
        transition: background-color 0.2s;
      }

      .test-detail-outer-summary::-webkit-details-marker {
        display: none;
      }

      .test-detail-outer-summary::after {
        content: "+";
        position: absolute;
        right: 20px;
        top: 50%;
        transform: translateY(-50%);
        font-size: 18px;
        font-weight: bold;
        color: #666;
      }

      .test-detail-outer-accordion[open] .test-detail-outer-summary::after {
        content: "−";
      }

      .test-detail-outer-summary:hover {
        background-color: #ebebeb;
      }

      .test-detail-outer-content {
        padding: 20px;
        background-color: white;
        border-top: 1px solid #ddd;
      }
    </style>

    ' as html;


          -- end
   
   
   
   
   
   
   --------------accordion end;

```


CMMC Level page

```sql ce/regime/cmmc_level.sql { route: { caption: "CMMC Level" } }


    SELECT 'title' AS component, (SELECT COALESCE(title, caption)
    FROM sqlpage_aide_navigation
   WHERE namespace = 'prime' AND path = 'ce/regime/cmmc_level.sql/index.sql') as contents;
    ;

    --- Breadcrumbs
    SELECT 'breadcrumb' AS component;
    SELECT 'Home' AS title, sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/' AS link;    
    SELECT 'CMMC' AS title, sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/ce/regime/cmmc.sql' AS link;
    SELECT 'CMMC Level ' || COALESCE(@level::TEXT,'') AS title, '#' AS link;

    --- Description text
    SELECT 'text' AS component,
       "The Cybersecurity Maturity Model Certification (CMMC) program aligns with the information security requirements of the U.S. Department of Defense (DoD) for Defense Industrial Base (DIB) partners. The DoD has mandated that all organizations engaged in business with them, irrespective of size, industry, or level of involvement, undergo a cybersecurity maturity assessment based on the CMMC framework. This initiative aims to ensure the protection of sensitive unclassified information shared between the Department and its contractors and subcontractors. The program enhances the Department's confidence that contractors and subcontractors adhere to cybersecurity requirements applicable to acquisition programs and systems handling controlled unclassified information" AS contents;


    --- Table (markdown column)
    SELECT 'table' AS component, TRUE AS sort, TRUE AS search, "Control Code" AS markdown;

    -- Pagination Controls (Top)
    SET total_rows = (SELECT COUNT(*) FROM scf_view 
      WHERE 
        (@level = 1 AND cmmc_level_1 IS NOT NULL AND cmmc_level_1 != '')
     OR (@level = 2 AND cmmc_level_2 IS NOT NULL AND cmmc_level_2 != '')
     OR (@level = 3 AND cmmc_level_3 IS NOT NULL AND cmmc_level_3 != '')
    );
SET limit = COALESCE($limit, 50);
SET offset = COALESCE($offset, 0);
SET total_pages = ($total_rows + $limit - 1) / $limit;
SET current_page = ($offset / $limit) + 1;

    --- Table data
    SELECT
      '[' || replace(replace(
          CASE 
            WHEN @level = 1 THEN cmmc_level_1
            WHEN @level = 2 THEN cmmc_level_2
            ELSE cmmc_level_3
          END,
          '
', ' '),
          '
', ' ')
|| '](' || sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/ce/regime/cmmc_detail.sql?code=' 
|| replace(replace( 
    CASE  
      WHEN @level = 1 THEN cmmc_level_1 
      WHEN @level = 2 THEN cmmc_level_2 
      ELSE cmmc_level_3 
    END, 
    '
', ' '), ' ', '%20') 
|| '&fiiid=' || replace(control_code, ' ', '%20')
|| '&level=' || @level
|| ')' AS "Control Code",

      scf_domain       AS "Domain",
      scf_control      AS "Title",
      control_code     AS "FII ID",
      control_description AS "Control Description",
      control_question AS "Question"

    FROM scf_view
    WHERE 
          (@level = 1 AND cmmc_level_1 IS NOT NULL AND cmmc_level_1 != '')
      OR (@level = 2 AND cmmc_level_2 IS NOT NULL AND cmmc_level_2 != '')
      OR (@level = 3 AND cmmc_level_3 IS NOT NULL AND cmmc_level_3 != '')
    ORDER BY control_code
    LIMIT $limit OFFSET $offset;

    -- Pagination Controls (Bottom)
    SELECT 'text' AS component,
    (SELECT CASE WHEN CAST($current_page AS INTEGER) > 1 THEN '[Previous](?limit=' || $limit || '&offset=' || ($offset - $limit) || '&level=' || replace($level, ' ', '%20') || ')' ELSE '' END)
    || ' '
    || '(Page ' || $current_page || ' of ' || $total_pages || ") "
    || (SELECT CASE WHEN CAST($current_page AS INTEGER) < CAST($total_pages AS INTEGER) THEN '[Next](?limit=' || $limit || '&offset=' || ($offset + $limit) || '&level=' || replace($level, ' ', '%20') || ')' ELSE '' END)
    AS contents_md
;      

```

CMMC Detail page

```sql ce/regime/cmmc_detail.sql { route: { caption: "CMMC Control Details" } }


  SELECT 'title' AS component, (SELECT COALESCE(title, caption)
    FROM sqlpage_aide_navigation
   WHERE namespace = 'prime' AND path = 'ce/regime/cmmc_detail.sql/index.sql') as contents;
    ;
  --- Breadcrumbs
  SELECT 'breadcrumb' AS component;
  SELECT 'Home' AS title, sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/' AS link;  
  SELECT 'CMMC' AS title, sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/ce/regime/cmmc.sql' AS link;
  SELECT 'CMMC Level ' || COALESCE($level::TEXT, '') AS title, sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/ce/regime/cmmc_level.sql?level=' || COALESCE($level::TEXT,'1') AS link;
  SELECT COALESCE($code, '') AS title, '#' AS link;

  

  --- Primary details card
  SELECT 'card' AS component, $page_title AS title, 1 AS columns;
  SELECT
      COALESCE($code, '(unknown)') AS title,
      '**Control Question:** ' || COALESCE(control_question, '') || '  

' ||
      '**Control Description:** ' || COALESCE(control_description, '') || '  

' ||
      '**SCF Domain:** ' || COALESCE(scf_domain, '') || '  

' ||
      '**SCF Control:** ' || COALESCE(scf_control, '') || '  

' ||
      '**FII IDs:** ' || COALESCE(control_code, '') AS description_md
      
  FROM scf_view
  WHERE
        ($level = 1 AND replace(replace(cmmc_level_1,'
',' '),'\r','') = $code)
    OR ($level = 2 AND replace(replace(cmmc_level_2,'
',' '),'\r','') = $code)
    OR ($level = 3 AND replace(replace(cmmc_level_3,'
',' '),'\r','') = $code)
  LIMIT 1;

  -- TODO Placeholder Card
  SELECT
    'card' AS component,
    1 AS columns;

  -- Policy Generator Prompt Accordion
  SELECT 'html' AS component,
  '<details class="test-detail-outer-accordion" open>
    <summary class="test-detail-outer-summary">
      Policy Generator Prompt 
  <br>
  Create tailored policies directly for <b>Control Code: ' || $code || '</b> &mdash; <b>Level: ' || $level || '</b>.
  The "Policy Generator Prompt" lets you transform abstract requirements into actionable, 
  written policies. Simply provide the relevant control or framework element, and the prompt
  will guide you in producing a policy that aligns with best practices, regulatory standards, 
  and organizational needs. This makes policy creation faster, consistent, and accessible—even 
  for teams without dedicated compliance writers.
    </summary>
    <div class="test-detail-outer-content">' AS html;

  SELECT 'card' as component, 1 as columns;
  SELECT
    '
' || p.body_text AS description_md
    FROM ai_ctxe_complaince_prompt p
   
    WHERE p.control_id = $code AND  p.documentType = 'Author Prompt' AND p.fii_id=$fiiid
    AND (
    ($level = 1 AND regime = 'CMMC' AND category_type='Level 1') OR
    ($level = 2 AND regime = 'CMMC' AND category_type='Level 2') OR
    ($level = 3 AND regime = 'CMMC' AND category_type='Level 3')
    );
   

  SELECT 'html' AS component,
    '</div></details>' AS html;

  -- Policy Audit Prompt Accordion
  SELECT 'html' AS component,
  '<details class="test-detail-outer-accordion" open>
    <summary class="test-detail-outer-summary">
      Policy Audit Prompt 
      <br>
      Ensure your policies stay effective and compliant with the "Policy Audit Prompt". These prompts are designed to help users critically evaluate existing policies against standards, frameworks, and internal expectations. By running an audit prompt, you can identify gaps, inconsistencies, or outdated language, and quickly adjust policies to remain audit-ready and regulator-approved. This gives your team a reliable tool for continuous policy improvement and compliance assurance.
    </summary>
    <div class="test-detail-outer-content">' AS html;

  SELECT 'card' as component, 1 as columns;
  SELECT
    '
' || p.body_text AS description_md
    FROM ai_ctxe_complaince_prompt p
    WHERE p.control_id = $code AND p.documentType = 'Audit Prompt' AND p.fii_id=$fiiid AND
   ( 
    ($level = 1 AND regime = 'CMMC' AND category_type='Level 1') OR
    ($level = 2 AND regime = 'CMMC' AND category_type='Level 2') OR
    ($level = 3 AND regime = 'CMMC' AND category_type='Level 3')
    );

  SELECT 'html' AS component,
    '</div></details>' AS html;

  -- Generated Policies Accordion
  SELECT 'html' AS component,
  '<details class="test-detail-outer-accordion" open>
    <summary class="test-detail-outer-summary">
      Generated Policies
      <br>
      The Generated Policies section showcases real examples of policies created using the "Policy Generator Prompt". These samples illustrate how high-level controls are translated into concrete, practical policy documents. Each generated policy highlights structure, clarity, and compliance alignment—making it easier for users to adapt and deploy them within their own organizations. Think of this as a living library of ready-to-use policy templates derived directly from controls.
    </summary>
    <div class="test-detail-outer-content">' AS html;

  SELECT 'card' as component, 1 as columns;
  SELECT
    '
' || p.body_text AS description_md
    FROM ai_ctxe_policy p
    WHERE p.control_id = $code AND p.fii_id=$fiiid
    
    AND 
    (($level = 1 AND regimeType = 'CMMC' AND category_type='Level 1') OR
    ($level = 2 AND regimeType = 'CMMC' AND category_type='Level 2') OR
    ($level = 3 AND regimeType = 'CMMC' AND category_type='Level 3')
    );

  SELECT 'html' AS component,
    '</div></details>' AS html;

  -- CSS Styles
  SELECT 'html' as component,
  '<style>
      tr.actualClass-passed td.State {
          color: green !important;
      }
       tr.actualClass-failed td.State {
          color: red !important;
      }
        tr.actualClass-passed td.Statealign-middle {
          color: green !important;
      }
        tr.actualClass-failed td.Statealign-middle {
          color: red !important;
      }
      
      .btn-list {
      display: flex;
      justify-content: flex-end;
      }
     h2.accordion-header button {
      font-weight: 700;
    }

    /* Test Detail Outer Accordion Styles */
    .test-detail-outer-accordion {
      border: 1px solid #ddd;
      border-radius: 8px;
      margin: 20px 0;
      overflow: hidden;
    }

    .test-detail-outer-summary {
      background-color: #f5f5f5;
      padding: 15px 20px;
      cursor: pointer;
      font-weight: 600;
      color: #333;
      border: none;
      outline: none;
      user-select: none;
      list-style: none;
      position: relative;
      transition: background-color 0.2s;
    }

    .test-detail-outer-summary::-webkit-details-marker {
      display: none;
    }

    .test-detail-outer-summary::after {
      content: "+";
      position: absolute;
      right: 20px;
      top: 50%;
      transform: translateY(-50%);
      font-size: 18px;
      font-weight: bold;
      color: #666;
    }

    .test-detail-outer-accordion[open] .test-detail-outer-summary::after {
      content: "−";
    }

    .test-detail-outer-summary:hover {
      background-color: #ebebeb;
    }

    .test-detail-outer-content {
      padding: 20px;
      background-color: white;
      border-top: 1px solid #ddd;
    }
  </style>' as html;

  --- Fallback if no exact match
  SELECT 'text' AS component,
        'No exact control found for code: ' || COALESCE($code,'(empty)') || '. Showing a fallback example for Level ' || COALESCE($level::TEXT,'1') || '.' AS contents
  WHERE NOT EXISTS (
      SELECT 1 FROM scf_view
      WHERE
            ($level = 1 AND replace(replace(cmmc_level_1,'
',' '),'\r','') = $code)
        OR ($level = 2 AND replace(replace(cmmc_level_2,'
',' '),'\r','') = $code)
        OR ($level = 3 AND replace(replace(cmmc_level_3,'
',' '),'\r','') = $code)
  );

  --- Example fallback card (optional)
  SELECT 'card' AS component, 'Fallback control' AS title, 1 AS columns
  WHERE NOT EXISTS (
      SELECT 1 FROM scf_view
      WHERE
            ($level = 1 AND replace(replace(cmmc_level_1,'
',' '),'\r','') = $code)
        OR ($level = 2 AND replace(replace(cmmc_level_2,'
',' '),'\r','') = $code)
        OR ($level = 3 AND replace(replace(cmmc_level_3,'
',' '),'\r','') = $code)
  );

```

Controls page

```sql ce/regime/controls.sql { route: { caption: "AICPA" } }

  SELECT 'title' AS component, (SELECT COALESCE(title, caption)
    FROM sqlpage_aide_navigation
   WHERE namespace = 'prime' AND path = 'ce/regime/controls.sql/index.sql') as contents;
    ;
  SELECT
  'text' AS component,
  ''|| $regimeType ||' Controls' AS title;
  SELECT
  description as contents FROM compliance_regime WHERE title = $regimeType::TEXT;
  SELECT
  'table' AS component,
  TRUE AS sort,
  TRUE AS search,
  "Control Code" AS markdown;
  SELECT '[' || control_code || ']('|| sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/ce/regime/control/control_detail.sql?id=' || control_code || '&regimeType='|| replace($regimeType,
" ", "%20")||')' AS "Control Code",
  scf_control AS "Title",
  scf_domain AS "Domain",
  control_description AS "Control Description",
  control_id AS "Requirements"
  FROM compliance_regime_control WHERE control_type=$regimeType::TEXT;

```

CMMC page

```sql ce/regime/cmmc.sql { route: { caption: "Cybersecurity Maturity Model Certification (CMMC)" } }

SELECT 'title' AS component, (SELECT COALESCE(title, caption)
    FROM sqlpage_aide_navigation
   WHERE namespace = 'prime' AND path = 'ce/regime/cmmc.sql/index.sql') as contents;
    ;
SELECT 'text' AS component, $page_title AS title;

SELECT
  "The Cybersecurity Maturity Model Certification (CMMC) program aligns with the information security requirements of the U.S. Department of Defense (DoD) for Defense Industrial Base (DIB) partners. The DoD has mandated that all organizations engaged in business with them, irrespective of size, industry, or level of involvement, undergo a cybersecurity maturity assessment based on the CMMC framework. This initiative aims to ensure the protection of sensitive unclassified information shared between the Department and its contractors and subcontractors. The program enhances the Department's confidence that contractors and subcontractors adhere to cybersecurity requirements applicable to acquisition programs and systems handling controlled unclassified information" AS contents;

SELECT 'card' AS component, '' AS title, 3 AS columns;

SELECT
  'CMMC Model 2.0 LEVEL 1' AS title,
  '**Geography**: US 

  **Source**: Department of Defense (DoD) 

  **Cybersecurity Maturity Model Certification (CMMC) - Level 1 (Foundational)** 

  **Version**: 2.0 

  **Published/Last Reviewed Date/Year**: 2021-11-04 00:00:00+00' AS description_md, 
  sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/ce/regime/cmmc_level.sql?level=1' AS link
UNION
SELECT
  'CMMC Model 2.0 LEVEL 2' AS title,
  '**Geography**: US 

  **Source**: Department of Defense (DoD) 

  **Cybersecurity Maturity Model Certification (CMMC) - Level 2 (Advanced)** 

  **Version**: 2.0 

  **Published/Last Reviewed Date/Year**: 2021-11-04 00:00:00+00' AS description_md, 
  sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/ce/regime/cmmc_level.sql?level=2'
UNION
SELECT
  'CMMC Model 2.0 LEVEL 3' AS title,
  '**Geography**: US 

  **Source**: Department of Defense (DoD) 

  **Cybersecurity Maturity Model Certification (CMMC) - Level 3 (Expert)** 

  **Version**: 2.0 

  **Published/Last Reviewed Date/Year**: 2021-11-04 00:00:00+00' AS description_md, 
  sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/ce/regime/cmmc_level.sql?level=3';

```

HIPAA page

```sql ce/regime/hipaa_security_rule.sql { route: { caption: "HIPAA" } }

SELECT 'title' AS component, (SELECT COALESCE(title, caption)
    FROM sqlpage_aide_navigation
   WHERE namespace = 'prime' AND path = 'ce/regime/hipaa_security_rule.sql/index.sql') as contents;
    ;
 
SELECT
  'text' AS component,
  $page_title AS title;
 
SELECT
  'The HIPAA define administrative, physical, and technical measures required to ensure the confidentiality, integrity, and availability of electronic protected health information (ePHI).' AS contents;
 
-- Pagination controls (top)
SET total_rows = (SELECT COUNT(*) FROM hipaa_security_rule_safeguards );
SET limit = COALESCE($limit, 50);
SET offset = COALESCE($offset, 0);
SET total_pages = ($total_rows + $limit - 1) / $limit;
SET current_page = ($offset / $limit) + 1;
 
SELECT
  'table' AS component,
  TRUE AS sort,
  TRUE AS search,
  "Control Code" AS markdown;
 
SELECT
  '[' || hipaa_security_rule_reference || '](' ||
    sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/ce/regime/hipaa_security_rule_detail.sql?id=' || hipaa_security_rule_reference || ')' AS "Control Code",
  common_criteria AS "Common Criteria",
  safeguard AS "Control Question",
  handled_by_nq AS "Handled by nQ",
  fii_id AS "FII ID"
FROM hipaa_security_rule_safeguards
ORDER BY hipaa_security_rule_reference
LIMIT $limit OFFSET $offset;
 
-- Pagination controls (bottom)
SELECT 'text' AS component,
    (SELECT CASE WHEN CAST($current_page AS INTEGER) > 1 THEN '[Previous](?limit=' || $limit || '&offset=' || ($offset - $limit) || ')' ELSE '' END)
    || ' '
    || '(Page ' || $current_page || ' of ' || $total_pages || ") "
    || (SELECT CASE WHEN CAST($current_page AS INTEGER) < CAST($total_pages AS INTEGER) THEN '[Next](?limit=' || $limit || '&offset=' || ($offset + $limit) || ')' ELSE '' END)
    AS contents_md
;      

```

HiTRUST e1 Assessment page

```sql ce/regime/hitrust.sql { route: { caption: "HiTRUST e1 Assessment" } }

SELECT 'title' AS component, (SELECT COALESCE(title, caption)
    FROM sqlpage_aide_navigation
   WHERE namespace = 'prime' AND path = 'ce/regime/hipaa_security_rule.sql/index.sql') as contents;
    ;
 
SELECT
  'text' AS component,
  'HIPAA' AS title;
 
SELECT
  'The HIPAA define administrative, physical, and technical measures required to ensure the confidentiality, integrity, and availability of electronic protected health information (ePHI).' AS contents;
 
-- Pagination controls (top)
SET total_rows = (SELECT COUNT(*) FROM hipaa_security_rule_safeguards );
SET limit = COALESCE($limit, 50);
SET offset = COALESCE($offset, 0);
SET total_pages = ($total_rows + $limit - 1) / $limit;
SET current_page = ($offset / $limit) + 1;
 
SELECT
  'table' AS component,
  TRUE AS sort,
  TRUE AS search,
  "Control Code" AS markdown;
 
SELECT
  '[' || hipaa_security_rule_reference || '](' ||
    sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/ce/regime/hipaa_security_rule_detail.sql?id=' || hipaa_security_rule_reference || ')' AS "Control Code",
  common_criteria AS "Common Criteria",
  safeguard AS "Control Question",
  handled_by_nq AS "Handled by nQ",
  fii_id AS "FII ID"
FROM hipaa_security_rule_safeguards
ORDER BY hipaa_security_rule_reference
LIMIT $limit OFFSET $offset;
 
-- Pagination controls (bottom)
SELECT 'text' AS component,
    (SELECT CASE WHEN CAST($current_page AS INTEGER) > 1 THEN '[Previous](?limit=' || $limit || '&offset=' || ($offset - $limit) || ')' ELSE '' END)
    || ' '
    || '(Page ' || $current_page || ' of ' || $total_pages || ") "
    || (SELECT CASE WHEN CAST($current_page AS INTEGER) < CAST($total_pages AS INTEGER) THEN '[Next](?limit=' || $limit || '&offset=' || ($offset + $limit) || ')' ELSE '' END)
    AS contents_md
;
        ;

```

ISO 27001 v3 Control Details page

```sql ce/regime/iso-27001.sql { route: { caption: "ISO 27001 v3 Control Details" } }

SELECT 'title' AS component, (SELECT COALESCE(title, caption)
    FROM sqlpage_aide_navigation
   WHERE namespace = 'prime' AND path = 'ce/regime/iso-27001.sql/index.sql') as contents;
    ;

--- Breadcrumbs
SELECT 'breadcrumb' AS component;
SELECT 'Home'     AS title, sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/'          AS link;
SELECT 'ISO 27001 v3' AS title, '#'                               AS link;

--- Description text
SELECT
  'text' AS component,
  'The ISO 27001 v3 controls are aligned with the Secure Controls Framework (SCF) to provide a comprehensive mapping of security requirements.' AS contents;

--- Pagination Controls (Top)
SET total_rows = (SELECT COUNT(*) FROM compliance_iso_27001_control );
SET limit = COALESCE($limit, 50);
SET offset = COALESCE($offset, 0);
SET total_pages = ($total_rows + $limit - 1) / $limit;
SET current_page = ($offset / $limit) + 1;

--- Table (markdown column for detail links)
SELECT
  'table' AS component,
  TRUE    AS sort,
  TRUE    AS search,
  "Control Code" AS markdown;

--- Table data
SELECT
  '[' || control_code || '](' || sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/ce/regime/iso-27001_detail.sql?code=' || replace(control_code, ' ', '%20') || ')' AS "Control Code",
  scf_domain        AS "SCF Domain",
  scf_control       AS "SCF Control",
  control_description AS "Control Description",
  control_question  AS "Control Question",
  evidence          AS "Evidence"
FROM compliance_iso_27001_control
ORDER BY control_code ASC
LIMIT $limit OFFSET $offset;

--- Pagination Controls (Bottom)
SELECT 'text' AS component,
    (SELECT CASE WHEN CAST($current_page AS INTEGER) > 1 THEN '[Previous](?limit=' || $limit || '&offset=' || ($offset - $limit) || ')' ELSE '' END)
    || ' '
    || '(Page ' || $current_page || ' of ' || $total_pages || ") "
    || (SELECT CASE WHEN CAST($current_page AS INTEGER) < CAST($total_pages AS INTEGER) THEN '[Next](?limit=' || $limit || '&offset=' || ($offset + $limit) || ')' ELSE '' END)
    AS contents_md
;

```


SCF page

```sql ce/regime/scf.sql { route: { caption: "SCF" } }


      SELECT 'title' AS component, (SELECT COALESCE(title, caption)
    FROM sqlpage_aide_navigation
   WHERE namespace = 'prime' AND path = 'ce/regime/scf.sql/index.sql') as contents;
    ;
      SELECT
    'text' AS component,
    'Compliance Explorer ' AS title;
    SELECT
    'The compliance explorer cover a wide range of standards and guidelines across different areas of cybersecurity and data protection. They include industry-specific standards, privacy regulations, and cybersecurity frameworks. Complying with these frameworks supports a strong cybersecurity stance and alignment with data protection laws.' as contents;
    SELECT
    'card' AS component,
    '' AS title,
    2 AS columns;
    SELECT
      title,
      '**Geography:** ' || geography || '  
' ||
      '**Source:** ' || source || '  
' ||
      '**Health Insurance Portability and Accountability Act (HIPAA)**' || '  
' ||
      '**Version:** ' || version || '  
' ||
      '**Published/Last Reviewed Date/Year:** ' || last_reviewed_date || '  
' ||
      '[**Detail View**](' || sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/ce/regime/controls.sql?regimeType=US%20HIPAA'|| ')' AS description_md
    FROM compliance_regime
    WHERE title = 'US HIPAA';

    SELECT
      title,
      '**Geography:** ' || geography || '  
' ||
      '**Source:** ' || source || '  
' ||
      '**Standard 800-53 rev4**' || '  
' ||
      '**Version:** ' || version || '  
' ||
      '**Published/Last Reviewed Date/Year:** ' || last_reviewed_date || '  
' ||
      '[**Detail View**](' || sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/ce/regime/controls.sql?regimeType=NIST' || ')' AS description_md
    FROM compliance_regime
    WHERE title = 'NIST';

```

SOC2 detail page

```sql ce/regime/soc2_detail.sql { route: { caption: "Controls" } }


    -- Breadcrumbs
    SELECT 'breadcrumb' AS component;
    SELECT 'Home' AS title, sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/' AS link;    
    SELECT 'AICPA' AS title, sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/ce/regime/aicpa.sql' AS link;
 
    -- SOC 2 Type breadcrumb
    SELECT
      CASE
        WHEN $type = 'soc2-type1' THEN 'SOC 2 Type I'
        WHEN $type = 'soc2-type2' THEN 'SOC 2 Type II'
        ELSE 'SOC 2'
      END AS title,
      CASE
        WHEN $type = 'soc2-type1' THEN sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/ce/regime/soc2_type1.sql'
        WHEN $type = 'soc2-type2' THEN sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/ce/regime/soc2_type2.sql'
        ELSE sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/ce/regime/aicpa.sql'
      END AS link;
 
    -- Last breadcrumb (dynamic control_id, non-clickable)
    SELECT
      control_id AS title, '#' AS link
    FROM (
      SELECT control_id
      FROM compliance_regime_control_soc2
      WHERE $type = 'soc2-type1' AND control_id = $id::TEXT
      UNION ALL
      SELECT control_id
      FROM aicpa_soc2_type2_controls
      WHERE $type = 'soc2-type2' AND control_id = $id::TEXT
    ) t
    LIMIT 1;
 
    -- Card Header
    SELECT 'card' AS component,
           CASE
             WHEN $type = 'soc2-type1' THEN 'SOC 2 Type I Control Detail'
             WHEN $type = 'soc2-type2' THEN 'SOC 2 Type II Control Detail'
             ELSE 'SOC 2 Control Detail'
           END AS title,
           1 AS columns;
 
    -- Detail Section (aligned UNION)
    SELECT
      common_criteria AS title,
      '**Control Code:** ' || control_id || '  

' ||
      '**Control Name:** ' || control_name || '  

' ||
      (CASE WHEN $type = 'soc2-type2' THEN '**FII ID:** ' || COALESCE(fii_id,'') || '  

' ELSE '' END) ||
      '**Control Question:** ' || COALESCE(control_question,'') || '  

'
      AS description_md
    FROM (
      -- Type I controls (with SCF reference)
      SELECT control_id, control_name, fii_id, common_criteria, control_question
      FROM compliance_regime_control_soc2
      WHERE $type = 'soc2-type1' AND control_id = $id::TEXT
     
      UNION ALL
     
      -- Type II controls (no SCF reference → add NULL for column alignment)
      SELECT control_id, control_name, fii_id, common_criteria, control_question
      FROM aicpa_soc2_type2_controls
      WHERE $type = 'soc2-type2' AND control_id = $id::TEXT
    );
    -- TODO Placeholder Card
    SELECT
      'card' AS component,
      1 AS columns;
 
 
   -----accordion start
   SELECT 'html' AS component,
  '<details class="test-detail-outer-accordion" open>
    <summary class="test-detail-outer-summary">
      Policy Generator Prompt 
  <br>
  Create tailored policies directly for <b>Control Code: ' || $id || '</b> &mdash; <b>FII ID: ' || fii_id || '</b>.
  The "Policy Generator Prompt" lets you transform abstract requirements into actionable, 
  written policies. Simply provide the relevant control or framework element, and the prompt
  will guide you in producing a policy that aligns with best practices, regulatory standards, 
  and organizational needs. This makes policy creation faster, consistent, and accessible—even 
  for teams without dedicated compliance writers.
    </summary>
    <div class="test-detail-outer-content">' AS html
FROM (SELECT control_id, fii_id
    FROM compliance_regime_control_soc2
    WHERE $type = 'soc2-type1' AND control_id = $id::TEXT
    
    UNION ALL
    
    SELECT control_id, fii_id
    FROM aicpa_soc2_type2_controls
    WHERE $type = 'soc2-type2' AND control_id = $id::TEXT
)

     
    SELECT 'card' as component, 1 as columns;
    SELECT
      '
' || p.body_text AS description_md
      FROM ai_ctxe_complaince_prompt p
      WHERE p.control_id = $id AND p.documentType = 'Author Prompt' AND (
    ($type = 'soc2-type1' AND regime = 'SOC2-TypeI') OR
    ($type = 'soc2-type2' AND regime = 'SOC2-TypeII')
  );
      

    
    SELECT 'html' AS component,
      '</div></details>' AS html;

      --accordion for audit prompt

SELECT 'html' AS component,
  '<details class="test-detail-outer-accordion" open>
    <summary class="test-detail-outer-summary">
      Policy Audit Prompt 
      <br>
      Ensure your policies stay effective and compliant with the "Policy Audit Prompt". These prompts are designed to help users critically evaluate existing policies against standards, frameworks, and internal expectations. By running an audit prompt, you can identify gaps, inconsistencies, or outdated language, and quickly adjust policies to remain audit-ready and regulator-approved. This gives your team a reliable tool for continuous policy improvement and compliance assurance.
    </summary>
    <div class="test-detail-outer-content">' AS html
FROM (SELECT control_id, fii_id
    FROM compliance_regime_control_soc2
    WHERE $type = 'soc2-type1' AND control_id = $id::TEXT
    
    UNION ALL
    
    SELECT control_id, fii_id
    FROM aicpa_soc2_type2_controls
    WHERE $type = 'soc2-type2' AND control_id = $id::TEXT
)

    SELECT 'card' as component, 1 as columns;
    SELECT
      '
' || p.body_text AS description_md
      FROM ai_ctxe_complaince_prompt p
      WHERE p.control_id = $id AND p.documentType = 'Audit Prompt' AND (
    ($type = 'soc2-type1' AND regime = 'SOC2-TypeI') OR
    ($type = 'soc2-type2' AND regime = 'SOC2-TypeII')
  );
      
 SELECT 'html' AS component,
      '</div></details>' AS html;

      
SELECT 'html' AS component,
  '<details class="test-detail-outer-accordion" open>
    <summary class="test-detail-outer-summary">
      Generated Policies
      <br>
      The Generated Policies section showcases real examples of policies created using the "Policy Generator Prompt". These samples illustrate how high-level controls are translated into concrete, practical policy documents. Each generated policy highlights structure, clarity, and compliance alignment—making it easier for users to adapt and deploy them within their own organizations. Think of this as a living library of ready-to-use policy templates derived directly from controls.
    </summary>
    <div class="test-detail-outer-content">' AS html
FROM (SELECT control_id, fii_id
    FROM compliance_regime_control_soc2
    WHERE $type = 'soc2-type1' AND control_id = $id::TEXT
    
    UNION ALL
    
    SELECT control_id, fii_id
    FROM aicpa_soc2_type2_controls
    WHERE $type = 'soc2-type2' AND control_id = $id::TEXT
)

    SELECT 'card' as component, 1 as columns;
    SELECT
      '
' || p.body_text AS description_md
      FROM ai_ctxe_policy p
      WHERE p.control_id = $id AND (
    ($type = 'soc2-type1' AND regimeType = 'SOC2-TypeI') OR
    ($type = 'soc2-type2' AND regimeType = 'SOC2-TypeII')
  );
   SELECT 'html' AS component,
      '</div></details>' AS html;
      SELECT 'html' as component,
    '<style>
        tr.actualClass-passed td.State {
            color: green !important; /* Default to red */
        }
         tr.actualClass-failed td.State {
            color: red !important; /* Default to red */
        }
          tr.actualClass-passed td.Statealign-middle {
            color: green !important; /* Default to red */
        }
          tr.actualClass-failed td.Statealign-middle {
            color: red !important; /* Default to red */
        }
        
        .btn-list {
        display: flex;
        justify-content: flex-end;
        }
       h2.accordion-header button {
        font-weight: 700;
      }

      /* Test Detail Outer Accordion Styles */
      .test-detail-outer-accordion {
        border: 1px solid #ddd;
        border-radius: 8px;
        margin: 20px 0;
        overflow: hidden;
      }

      .test-detail-outer-summary {
        background-color: #f5f5f5;
        padding: 15px 20px;
        cursor: pointer;
        font-weight: 600;
        color: #333;
        border: none;
        outline: none;
        user-select: none;
        list-style: none;
        position: relative;
        transition: background-color 0.2s;
      }

      .test-detail-outer-summary::-webkit-details-marker {
        display: none;
      }

      .test-detail-outer-summary::after {
        content: "+";
        position: absolute;
        right: 20px;
        top: 50%;
        transform: translateY(-50%);
        font-size: 18px;
        font-weight: bold;
        color: #666;
      }

      .test-detail-outer-accordion[open] .test-detail-outer-summary::after {
        content: "−";
      }

      .test-detail-outer-summary:hover {
        background-color: #ebebeb;
      }

      .test-detail-outer-content {
        padding: 20px;
        background-color: white;
        border-top: 1px solid #ddd;
      }
    </style>

    ' as html;


          -- end
   
   
   
   
   
   
   --------------accordion end;

```



The below SQL code first drops the table if it already exists, then defines columns for key attributes such as title, geography, source, version, review date, description, status, and JSON-based elaboration. It includes audit fields like created_at, updated_at, and deleted_at for tracking changes.

```sql HEAD
-- Drop the table if it exists, then create the new table with auto-increment primary key
DROP TABLE IF EXISTS "compliance_regime";
CREATE TABLE "compliance_regime" (
"compliance_regime_id" INTEGER PRIMARY KEY AUTOINCREMENT,
"title" TEXT NOT NULL,
"geography" TEXT,
"source" TEXT,
"description" TEXT,
"logo" TEXT,
"status" TEXT,
"version" TEXT,
"last_reviewed_date" TIMESTAMPTZ,
"authoritative_source" TEXT,
"custom_user_text" TEXT,
"elaboration" TEXT CHECK(json_valid(elaboration) OR elaboration IS NULL),
"created_at" TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP,
"created_by" TEXT DEFAULT 'UNKNOWN',
"updated_at" TIMESTAMPTZ,
"updated_by" TEXT,
"deleted_at" TIMESTAMPTZ,
"deleted_by" TEXT,
"activity_log" TEXT
);
-- Insert records into the table
INSERT INTO "compliance_regime" (
"title",
"geography",
"source",
"description",
"logo",
"status",
"version",
"last_reviewed_date",
"authoritative_source",
"custom_user_text"
)
VALUES
(
'HIPAA',
'US',
'Federal',
'Health Insurance Portability and Accountability Act',
'',
'active',
'N/A',
'2022-10-20 00:00:00+00',
'Health Insurance Portability and Accountability Act (HIPAA)',
'Below, you will find a complete list of all controls applicable to the US HIPAA framework. These controls are designed ' ||
'to ensure compliance with the Health Insurance Portability and Accountability Act (HIPAA) standards, safeguarding ' ||
'sensitive patient health information'
),
(
'NIST',
'Universal',
'SCF',
'Comprehensive cybersecurity guidance framework',
'',
'active',
'2024',
'2024-04-01 00:00:00+00',
'800-53 rev4',
NULL
),
(
'SOC2 Type I',
'US',
'SCF',
'Report on Controls as a Service Organization. Relevant to Security, Availability, Processing Integrity, Confidentiality, or Privacy.',
'',
'active',
'2024',
'2024-04-01 00:00:00+00',
'800-53 rev4',
NULL
),(
'SOC2 Type II',
'US',
'SCF',
'SOC 2 Type II reports provide lists of Internal controls that are audited by an Independent third-party to show how well those controls are implemented and operating.',
'',
'active',
'2024',
'2024-04-01 00:00:00+00',
'800-53 rev4',
NULL
),(
'HITRUST CSF',
'US',
'SCF',
'Achieve HITRUST CSF certification, the most trusted and comprehensive security framework in healthcare.',
'',
'active',
'2024',
'2024-04-01 00:00:00+00',
'800-53 rev4',
NULL
),(
'CMMC Model 2.0 LEVEL 1',
'US',
'SCF',
'Achieve Cybersecurity Maturity Model Certification (CMMC) to bid on Department of Defense contracts',
'',
'active',
'2024',
'2024-04-01 00:00:00+00',
'800-53 rev4',
NULL
),(
'CMMC Model 2.0 LEVEL 2',
'US',
'SCF',
'110 requirements aligned with NIST SP 800-171; Triennial third-party assessment & annual affirmation; Triennial self-assessment & annual affirmation for select programs. A subset of programs with Level 2 requirements do not involve information critical to national security, and associated contractors will be permitted to meet the requirement through self-assessments. Contractors will be required to conduct self-assessment on an annual basis, accompanied by an annual affirmation from a senior company official that the company is meeting requirements. The Department intends to require companies to register self-assessments and affirmations in the Supplier Performance Risk System (SPRS).',
'',
'active',
'2024',
'2024-04-01 00:00:00+00',
'800-53 rev4',
NULL
),(
'CMMC Model 2.0 LEVEL 3',
'US',
'SCF',
'110+ requirements based on NIST SP 800-171 & 800-172; Triennial government-led assessment & annual affirmation. The Department intends for Level 3 cybersecurity requirements to be assessed by government officials. Assessment requirements are currently under development. Level 3 information will likewise be posted as it becomes available.',
'',
'active',
'2024',
'2024-04-01 00:00:00+00',
'800-53 rev4',
NULL
),(
'ISO 27001:2022',
'US',
'SCF',
'Information security management systems standard',
'',
'active',
'2024',
'2024-04-01 00:00:00+00',
'800-53 rev4',
NULL
);
```

These are the views to consolidate compliance controls from multiple frameworks (SCF, CMMC, HIPAA, HITRUST, ISO 27001, SOC2) and AI policy/prompt data. It standardizes fields like control codes, questions, evidence, and metadata, providing unified, queryable views for a Compliance Explorer platform.

```sql HEAD
DROP VIEW IF EXISTS compliance_regime_control;
CREATE VIEW compliance_regime_control AS
SELECT `SCF Domain` AS scf_domain,
`SCF Control` AS scf_control,
`Secure Controls Framework (SCF) Control Description` AS control_description,
`SCF Control Question` AS control_question,
"SCF #" AS control_code,
`US HIPAA` AS control_id,
'US HIPAA' AS control_type
FROM uniform_resource_scf_2024_2 WHERE `US HIPAA` !=''
UNION
SELECT `SCF Domain` AS scf_domain,
`SCF Control` AS scf_control,
`Secure Controls Framework (SCF) Control Description` AS control_description,
`SCF Control Question` AS control_question,
"SCF #" AS control_code,
`NIST 800-171A rev 3` AS control_id,
'NIST' AS control_type
FROM uniform_resource_scf_2024_2 WHERE `NIST 800-171A rev 3` !='';


DROP VIEW IF EXISTS scf_view;

CREATE VIEW scf_view AS
SELECT 
    'SCF-' || ROWID AS control_identifier,
    "SCF Domain" AS scf_domain,
    "SCF Control" AS scf_control,
    "SCF #" AS control_code,
    "Secure Controls Framework (SCF) Control Description" AS control_description,
    "SCF Control Question" AS control_question,
    "US CMMC 2.0 Level 1" AS cmmc_level_1,
    "US CMMC 2.0 Level 2" AS cmmc_level_2,
    "US CMMC 2.0 Level 3" AS cmmc_level_3
FROM uniform_resource_scf_2024_2;

DROP VIEW IF EXISTS ai_ctxe_policy;
CREATE VIEW ai_ctxe_policy AS
SELECT DISTINCT
  ur.uniform_resource_id,
  json_extract(ur.frontmatter, '$.title') AS title,
  json_extract(ur.frontmatter, '$.description') AS description,
  json_extract(ur.frontmatter, '$.publishDate') AS publishDate,
  json_extract(ur.frontmatter, '$.publishBy') AS publishBy,
  json_extract(ur.frontmatter, '$.classification') AS classification,
  json_extract(ur.frontmatter, '$.documentType') AS documentType,
  json_extract(ur.frontmatter, '$.approvedBy') AS approvedBy,
  json_extract(ur.frontmatter, '$.category') AS category,
  json_extract(ur.frontmatter, '$.control-id') AS control_id,
  json_extract(ur.frontmatter, '$.regimeType') AS regimeType,
  json_extract(ur.frontmatter, '$.category[1]') AS category_type,
  json_extract(ur.frontmatter,'$.fiiId') AS fii_id,
 
  TRIM(
    CASE
      WHEN instr(ur.content, '---') = 1
        THEN substr(
          ur.content,
          instr(ur.content, '---') + 3 + instr(substr(ur.content, instr(ur.content, '---') + 3), '---') + 3
        )
      ELSE ur.content
    END
  ) AS body_text
FROM
  uniform_resource ur
JOIN
  ur_ingest_session_fs_path_entry fs
    ON fs.uniform_resource_id = ur.uniform_resource_id

WHERE
  fs.file_basename LIKE '%.policy.md';

DROP VIEW IF EXISTS compliance_regime_control_soc2;

CREATE VIEW compliance_regime_control_soc2 AS
SELECT
  "#" AS control_code,
  "Control Identifier" AS control_id,
  "Fii ID" AS fii_id,
  "Common Criteria" AS common_criteria,
  "Common Criteria type" AS criteria_type,
  Name AS control_name,
  "Questions Descriptions" AS control_question,
  'AICPA SOC 2' AS control_type,
  tenant_id,
  tenant_name
FROM uniform_resource_aicpa_soc2_controls
WHERE "Control Identifier" IS NOT NULL AND "Control Identifier" != '';


DROP VIEW IF EXISTS compliance_regime_control_hitrust_e1;

CREATE VIEW compliance_regime_control_hitrust_e1 AS
SELECT
  "#" AS control_code,
  "Control Identifier" AS control_id,
  "Fii ID" AS fii_id,
  "Common Criteria" AS common_criteria,
  NULL AS criteria_type, -- not available in this table
  Name AS control_name,
  Description AS control_question,
  'HITRUST E1' AS control_type,
  tenant_id,
  tenant_name
FROM uniform_resource_hitrust_e1_assessment
WHERE "Control Identifier" IS NOT NULL 
  AND "Control Identifier" != '';

DROP VIEW IF EXISTS compliance_iso_27001_control;

CREATE VIEW compliance_iso_27001_control AS
SELECT 
    `SCF Domain` AS scf_domain,
    `SCF Control` AS scf_control,
    `SCF #` AS control_code,
    `Secure Controls Framework (SCF) Control Description` AS control_description,
    `SCF Control Question` AS control_question,
    Evidence AS evidence,
    tenant_id,
    tenant_name,
    'ISO 27001 v3' AS control_type
FROM uniform_resource_iso_27001_v3;

DROP VIEW IF EXISTS hipaa_security_rule_safeguards;
CREATE VIEW hipaa_security_rule_safeguards AS
SELECT
    "#" AS id,
    "Common Criteria" AS common_criteria,
    "HIPAA Security Rule Reference" AS hipaa_security_rule_reference,
    Safeguard AS safeguard,
    "Handled by nQ" AS handled_by_nq,
    "FII Id" AS fii_id,
    tenant_id,
    tenant_name
FROM uniform_resource_hipaa_security_rule_safeguards;
 
DROP VIEW IF EXISTS compliance_regime_thsa;
CREATE VIEW compliance_regime_thsa AS
SELECT
   "#" AS id,
  `SCF Domain` AS scf_domain,
  `SCF Control` AS scf_control,
  `SCF Control Question` AS scf_control_question,
  "SCF #" AS scf_code,
  "Your Answer" AS your_answer,
  tenant_id,
  tenant_name
FROM uniform_resource_thsa;


DROP VIEW IF EXISTS aicpa_soc2_type2_controls;
CREATE VIEW aicpa_soc2_type2_controls AS
SELECT
    "#" AS id,
    "Control Identifier" AS control_id,
    "Fii ID" AS fii_id,
    "Common Criteria" AS common_criteria,
    "Common Criteria type" AS criteria_type,
    Name AS control_name,
    "Questions Descriptions" AS control_question,
    tenant_id,
    tenant_name
FROM uniform_resource_aicpa_soc2_type2_controls;

-- View for compliance explorer prompts

DROP VIEW IF EXISTS ai_ctxe_complaince_prompt;
CREATE VIEW ai_ctxe_complaince_prompt AS
SELECT DISTINCT
  ur.uniform_resource_id,
  json_extract(ur.frontmatter, '$.title') AS title,
  json_extract(ur.frontmatter, '$.description') AS description,
  json_extract(ur.frontmatter, '$.publishDate') AS publishDate,
  json_extract(ur.frontmatter, '$.publishBy') AS publishBy,
  json_extract(ur.frontmatter, '$.classification') AS classification,
  json_extract(ur.frontmatter, '$.documentType') AS documentType,
  json_extract(ur.frontmatter, '$.approvedBy') AS approvedBy,
  json_extract(ur.frontmatter, '$.category') AS category,
  json_extract(ur.frontmatter, '$.control-id') AS control_id,
  json_extract(ur.frontmatter, '$.regimeType') AS regime,
  json_extract(ur.frontmatter, '$.category[1]') AS category_type,
  json_extract(ur.frontmatter,'$.fiiId') AS fii_id,

  TRIM(
    CASE
      WHEN instr(ur.content, '---') = 1
        THEN substr(
          ur.content,
          instr(ur.content, '---') + 3 + instr(substr(ur.content, instr(ur.content, '---') + 3), '---') + 3
        )
      ELSE ur.content
    END
  ) AS body_text
FROM
  uniform_resource ur
JOIN
  ur_ingest_session_fs_path_entry fs
    ON fs.uniform_resource_id = ur.uniform_resource_id

WHERE
  fs.file_basename LIKE '%.prompt.md'
  AND json_extract(ur.frontmatter, '$.regimeType') IS NOT NULL;


-- View for all controls details compliance explorer

DROP VIEW IF EXISTS all_control;

CREATE VIEW all_control AS
    SELECT
    (SELECT COUNT(*)
     FROM uniform_resource_scf_2024_2 AS sub
     WHERE sub.ROWID <= cntl.ROWID
       AND "US CMMC 2.0 Level 1" != '') AS display_order,
    'CMMCLEVEL-' || ROWID AS control_identifier,
    cntl."US CMMC 2.0 Level 1" AS control_code,
    cntl."SCF #" AS fii,
    cntl."SCF Domain" AS common_criteria,
    '' AS expected_evidence,
    cntl."SCF Control Question" AS question,
    'CMMC Model 2.0 Level 1' AS control_type,
    12 AS control_type_id,
    6 AS control_compliance_id
FROM uniform_resource_scf_2024_2 AS cntl
WHERE cntl."US CMMC 2.0 Level 1" != ''
 
UNION ALL
SELECT
    (SELECT COUNT(*)
     FROM uniform_resource_scf_2024_2 AS sub
     WHERE sub.ROWID <= cntl.ROWID
       AND "US CMMC 2.0 Level 2" != '') AS display_order,
    'CMMCLEVEL-' || ROWID AS control_identifier,
    cntl."US CMMC 2.0 Level 2" AS control_code,
    cntl."SCF #" AS fii,
    cntl."SCF Domain" AS common_criteria,
    '' AS expected_evidence,
    cntl."SCF Control Question" AS question,
    'CMMC Model 2.0 Level 2' AS control_type,
    13 AS control_type_id,
    7 AS control_compliance_id
FROM uniform_resource_scf_2024_2 AS cntl
WHERE cntl."US CMMC 2.0 Level 2" != ''
 
UNION ALL
SELECT
    (SELECT COUNT(*)
     FROM uniform_resource_scf_2024_2 AS sub
     WHERE sub.ROWID <= cntl.ROWID
       AND "US CMMC 2.0 Level 3" != '') AS display_order,
    'CMMCLEVEL-' || ROWID AS control_identifier,
    cntl."US CMMC 2.0 Level 3" AS control_code,
    cntl."SCF #" AS fii,
    cntl."SCF Domain" AS common_criteria,
    '' AS expected_evidence,
    cntl."SCF Control Question" AS question,
    'CMMC Model 2.0 Level 3' AS control_type,
    14 AS control_type_id,
    8 AS control_compliance_id
FROM uniform_resource_scf_2024_2 AS cntl
WHERE cntl."US CMMC 2.0 Level 3" != ''
 
UNION ALL
 
SELECT
            CAST(cntl."#" AS INTEGER) AS display_order,
            cntl."HIPAA Security Rule Reference" AS control_identifier,
            cntl."HIPAA Security Rule Reference" AS control_code,
            cntl."FII Id" AS fii,
            cntl."Common Criteria" AS common_criteria,
            '' AS expected_evidence,
            cntl.Safeguard AS question,
            'HIPAA' AS control_type,
            0 AS control_type_id,
            1 AS control_compliance_id        
          FROM uniform_resource_hipaa_security_rule_safeguards cntl
          
UNION ALL
SELECT
            CAST(cntl."#" AS INTEGER) AS display_order,
            cntl."Control Identifier" AS control_identifier,
            cntl."Control Identifier" AS control_code,
            cntl."Fii ID" AS fii,
            cntl."Common Criteria" AS common_criteria,
            cntl."Name" AS expected_evidence,
            cntl.Description AS question,
            'HITRUST' AS control_type,
            0 AS control_type_id,
            5 AS control_compliance_id  
          FROM uniform_resource_hitrust_e1_assessment cntl
          
UNION ALL
SELECT
            (SELECT COUNT(*)
            FROM uniform_resource_iso_27001_v3 AS sub
            WHERE sub.ROWID <= cntl.ROWID) AS display_order,
            'ISO-27001-' || (ROWID) as control_identifier,
             cntl."SCF #" AS control_code,
             cntl."SCF #" AS fii,
             cntl."SCF Domain" AS common_criteria,
             Evidence as expected_evidence,
             cntl."SCF Control Question" AS question,
             'ISO 27001:2022' AS control_type,
            0 AS control_type_id,
             9 AS control_compliance_id          
        FROM uniform_resource_iso_27001_v3 as cntl
UNION ALL
SELECT
        CAST(cntl."#" AS INTEGER) AS display_order,
        cntl."Control Identifier" AS control_identifier,
        cntl."Control Identifier" AS control_code,
        cntl."Fii ID" AS fii,
        cntl."Common Criteria" AS common_criteria,
        cntl."Name" AS expected_evidence,
        cntl."Questions Descriptions" AS question,
        'SOC2 Type I' AS control_type,
        2 AS control_type_id,
        3 AS control_compliance_id
    FROM uniform_resource_aicpa_soc2_controls cntl
    UNION ALL
    SELECT
        CAST(cntl."#" AS INTEGER),
        cntl."Control Identifier",
        cntl."Control Identifier",
        cntl."Fii ID",
        cntl."Common Criteria",
        cntl."Name",
        cntl."Questions Descriptions",
        'SOC2 Type II' AS control_type,
        3 AS control_type_id,
        4 AS control_compliance_id  
    FROM uniform_resource_aicpa_soc2_type2_controls cntl;


-- View for CMMC controls details compliance explorer

DROP VIEW IF EXISTS cmmc_control;

CREATE VIEW cmmc_control AS
    SELECT
    (SELECT COUNT(*)
     FROM uniform_resource_scf_2024_2 AS sub
     WHERE sub.ROWID <= cntl.ROWID
       AND "US CMMC 2.0 Level 1" != '') AS display_order,
    'CMMCLEVEL-' || ROWID AS control_identifier,
    cntl."US CMMC 2.0 Level 1" AS control_code,
    cntl."SCF #" AS fii,
    cntl."SCF Domain" AS common_criteria,
    '' AS expected_evidence,
    cntl."SCF Control Question" AS question,
    'CMMC Model 2.0 Level 1' AS control_type,
    12 AS control_type_id
FROM uniform_resource_scf_2024_2 AS cntl
WHERE cntl."US CMMC 2.0 Level 1" != ''
 
UNION ALL
SELECT
    (SELECT COUNT(*)
     FROM uniform_resource_scf_2024_2 AS sub
     WHERE sub.ROWID <= cntl.ROWID
       AND "US CMMC 2.0 Level 2" != '') AS display_order,
    'CMMCLEVEL-' || ROWID AS control_identifier,
    cntl."US CMMC 2.0 Level 2" AS control_code,
    cntl."SCF #" AS fii,
    cntl."SCF Domain" AS common_criteria,
    '' AS expected_evidence,
    cntl."SCF Control Question" AS question,
    'CMMC Model 2.0 Level 2' AS control_type,
    13 AS control_type_id
FROM uniform_resource_scf_2024_2 AS cntl
WHERE cntl."US CMMC 2.0 Level 2" != ''
 
UNION ALL
SELECT
    (SELECT COUNT(*)
     FROM uniform_resource_scf_2024_2 AS sub
     WHERE sub.ROWID <= cntl.ROWID
       AND "US CMMC 2.0 Level 3" != '') AS display_order,
    'CMMCLEVEL-' || ROWID AS control_identifier,
    cntl."US CMMC 2.0 Level 3" AS control_code,
    cntl."SCF #" AS fii,
    cntl."SCF Domain" AS common_criteria,
    '' AS expected_evidence,
    cntl."SCF Control Question" AS question,
    'CMMC Model 2.0 Level 3' AS control_type,
    14 AS control_type_id
FROM uniform_resource_scf_2024_2 AS cntl
WHERE cntl."US CMMC 2.0 Level 3" != '';


-- View for HIPAA controls details compliance explorer

DROP VIEW IF EXISTS hipaa_control;

CREATE VIEW hipaa_control AS
   SELECT
            CAST(cntl."#" AS INTEGER) AS display_order,
            cntl."HIPAA Security Rule Reference" AS control_identifier,
            cntl."HIPAA Security Rule Reference" AS control_code,
            cntl."FII Id" AS fii,
            cntl."Common Criteria" AS common_criteria,
            '' AS expected_evidence,
            cntl.Safeguard AS question            
          FROM uniform_resource_hipaa_security_rule_safeguards cntl;


-- View for HITRUST controls details compliance explorer

DROP VIEW IF EXISTS hitrust_control;

CREATE VIEW hitrust_control as
SELECT
            CAST(cntl."#" AS INTEGER) AS display_order,
            cntl."Control Identifier" AS control_identifier,
            cntl."Control Identifier" AS control_code,
            cntl."Fii ID" AS fii,
            cntl."Common Criteria" AS common_criteria,
            cntl."Name" AS expected_evidence,
            cntl.Description AS question
          FROM uniform_resource_hitrust_e1_assessment cntl;


-- View for ISO 27001 controls details compliance explorer

DROP VIEW IF EXISTS iso27001_control;

CREATE VIEW iso27001_control AS    
SELECT
            (SELECT COUNT(*)
            FROM uniform_resource_iso_27001_v3 AS sub
            WHERE sub.ROWID <= cntl.ROWID) AS display_order,
            'ISO-27001-' || (ROWID) as control_identifier,
             cntl."SCF #" AS control_code,
             cntl."SCF #" AS fii,
             cntl."SCF Domain" AS common_criteria,
             Evidence as expected_evidence,
             cntl."SCF Control Question" AS question            
        FROM uniform_resource_iso_27001_v3 as cntl;


-- View for SOC2 controls details compliance explorer

DROP VIEW IF EXISTS soc2_control;

CREATE VIEW soc2_control AS
    SELECT
        CAST(cntl."#" AS INTEGER) AS display_order,
        cntl."Control Identifier" AS control_identifier,
        cntl."Control Identifier" AS control_code,
        cntl."Fii ID" AS fii,
        cntl."Common Criteria" AS common_criteria,
        cntl."Name" AS expected_evidence,
        cntl."Questions Descriptions" AS question,
        'SOC2 Type I' AS control_type,
        2 AS control_type_id
    FROM uniform_resource_aicpa_soc2_controls cntl
    UNION ALL
    SELECT
        CAST(cntl."#" AS INTEGER),
        cntl."Control Identifier",
        cntl."Control Identifier",
        cntl."Fii ID",
        cntl."Common Criteria",
        cntl."Name",
        cntl."Questions Descriptions",
        'SOC2 Type II' AS control_type,
        3 AS control_type_id
    FROM uniform_resource_aicpa_soc2_type2_controls cntl;       
```

HIPAA Security Rule Detail page

```sql ce/regime/hipaa_security_rule_detail.sql { route: { caption: "HIPAA Security Rule Detail" } }
SET resource_json = sqlpage.read_file_as_text('spry.d/auto/resource/${ctx.path}.auto.json');


      SELECT
        'breadcrumb' AS component;
  
      SELECT
        'Home' AS title,
         '/' AS link;

      SELECT
        'HIPAA' AS title,
         '/ce/regime/hipaa_security_rule.sql' AS link;
 
      -- Dynamic last breadcrumb using the reference from the DB
      SELECT
        hipaa_security_rule_reference AS title,
        '#' AS link
      FROM hipaa_security_rule_safeguards
      WHERE hipaa_security_rule_reference = $id::TEXT;
  
      SELECT
        'card' AS component,
        $page_title AS title,
        1 AS columns;
  
      SELECT
        common_criteria AS title,
        '**Control Code:** ' || hipaa_security_rule_reference || '  

' ||
        '**Control Question:** ' || safeguard || '  

' ||
        '**FII ID:** ' || fii_id || '  

'  AS description_md
      FROM hipaa_security_rule_safeguards
      WHERE hipaa_security_rule_reference = $id::TEXT;

      -- TODO Placeholder Card
    SELECT
      'card' AS component,
      1 AS columns;
 
          -- accordion for policy generator, audit prompt, and generated policies

              
   SELECT 'html' AS component,
  '<details class="test-detail-outer-accordion" open>
    <summary class="test-detail-outer-summary">
      Policy Generator Prompt 
  <br>
  Create tailored policies directly for <b>Control Code: ' || hipaa_security_rule_reference || '</b> &mdash; <b>FII ID: ' || fii_id || '</b>.
  The "Policy Generator Prompt" lets you transform abstract requirements into actionable, 
  written policies. Simply provide the relevant control or framework element, and the prompt
  will guide you in producing a policy that aligns with best practices, regulatory standards, 
  and organizational needs. This makes policy creation faster, consistent, and accessible—even 
  for teams without dedicated compliance writers.
    </summary>
    <div class="test-detail-outer-content">' AS html
FROM hipaa_security_rule_safeguards
WHERE hipaa_security_rule_reference = $id::TEXT;

     
    SELECT 'card' as component, 1 as columns;
    SELECT
      '
' || p.body_text AS description_md
      FROM ai_ctxe_complaince_prompt p
      WHERE p.control_id = $id AND p.documentType = 'Author Prompt'
      ;

    
    SELECT 'html' AS component,
      '</div></details>' AS html;

      --accordion for audit prompt

SELECT 'html' AS component,
  '<details class="test-detail-outer-accordion" open>
    <summary class="test-detail-outer-summary">
      Policy Audit Prompt 
      <br>
      Ensure your policies stay effective and compliant with the "Policy Audit Prompt". These prompts are designed to help users critically evaluate existing policies against standards, frameworks, and internal expectations. By running an audit prompt, you can identify gaps, inconsistencies, or outdated language, and quickly adjust policies to remain audit-ready and regulator-approved. This gives your team a reliable tool for continuous policy improvement and compliance assurance.
    </summary>
    <div class="test-detail-outer-content">' AS html
FROM hipaa_security_rule_safeguards
WHERE hipaa_security_rule_reference = $id::TEXT;

    SELECT 'card' as component, 1 as columns;
    SELECT
      '
' || p.body_text AS description_md
      FROM ai_ctxe_complaince_prompt p
      WHERE p.control_id = $id AND p.documentType = 'Audit Prompt'
      ;
 SELECT 'html' AS component,
      '</div></details>' AS html;

      
SELECT 'html' AS component,
  '<details class="test-detail-outer-accordion" open>
    <summary class="test-detail-outer-summary">
      Generated Policies
      <br>
      The Generated Policies section showcases real examples of policies created using the "Policy Generator Prompt". These samples illustrate how high-level controls are translated into concrete, practical policy documents. Each generated policy highlights structure, clarity, and compliance alignment—making it easier for users to adapt and deploy them within their own organizations. Think of this as a living library of ready-to-use policy templates derived directly from controls.
    </summary>
    <div class="test-detail-outer-content">' AS html
FROM hipaa_security_rule_safeguards
WHERE hipaa_security_rule_reference = $id::TEXT;

    SELECT 'card' as component, 1 as columns;
    SELECT
      '
' || p.body_text AS description_md
      FROM ai_ctxe_policy p
      WHERE p.control_id = $id;
   SELECT 'html' AS component,
      '</div></details>' AS html;
      SELECT 'html' as component,
    '<style>
        tr.actualClass-passed td.State {
            color: green !important; /* Default to red */
        }
         tr.actualClass-failed td.State {
            color: red !important; /* Default to red */
        }
          tr.actualClass-passed td.Statealign-middle {
            color: green !important; /* Default to red */
        }
          tr.actualClass-failed td.Statealign-middle {
            color: red !important; /* Default to red */
        }
        
        .btn-list {
        display: flex;
        justify-content: flex-end;
        }
       h2.accordion-header button {
        font-weight: 700;
      }

      /* Test Detail Outer Accordion Styles */
      .test-detail-outer-accordion {
        border: 1px solid #ddd;
        border-radius: 8px;
        margin: 20px 0;
        overflow: hidden;
      }

      .test-detail-outer-summary {
        background-color: #f5f5f5;
        padding: 15px 20px;
        cursor: pointer;
        font-weight: 600;
        color: #333;
        border: none;
        outline: none;
        user-select: none;
        list-style: none;
        position: relative;
        transition: background-color 0.2s;
      }

      .test-detail-outer-summary::-webkit-details-marker {
        display: none;
      }

      .test-detail-outer-summary::after {
        content: "+";
        position: absolute;
        right: 20px;
        top: 50%;
        transform: translateY(-50%);
        font-size: 18px;
        font-weight: bold;
        color: #666;
      }

      .test-detail-outer-accordion[open] .test-detail-outer-summary::after {
        content: "−";
      }

      .test-detail-outer-summary:hover {
        background-color: #ebebeb;
      }

      .test-detail-outer-content {
        padding: 20px;
        background-color: white;
        border-top: 1px solid #ddd;
      }
    </style>

    ' as html;


          -- end;

```

HiTRUST Control Details page

```sql ce/regime/hitrust_detail.sql { route: { caption: "HiTRUST Control Details" } }
SET resource_json = sqlpage.read_file_as_text('spry.d/auto/resource/${ctx.path}.auto.json');


    --- Breadcrumbs
    SELECT 'breadcrumb' AS component;
    SELECT 'Home' AS title,  '/' AS link;    
    SELECT 'HiTRUST e1 Assessment' AS title,  '/ce/regime/hitrust.sql' AS link;
    SELECT COALESCE($code, '') AS title, '#' AS link;

    --- Primary details card
    SELECT 'card' AS component, $page_title AS title, 1 AS columns;
    SELECT
        COALESCE(control_id, '(unknown)') AS title,
        '**Common Criteria:** ' || COALESCE(common_criteria,'') || '  

' ||
        '**Control Name:** ' || COALESCE(control_name,'') || '  

' ||
        '**Control Description:** ' || COALESCE(control_question,'') || '  

' ||
        '**FII ID:** ' || COALESCE(fii_id,'') AS description_md
    FROM compliance_regime_control_hitrust_e1
    WHERE control_id = $code
    LIMIT 1;

    -- TODO Placeholder Card
    SELECT
      'card' AS component,
      1 AS columns;

      SELECT 'html' AS component,
  '<details class="test-detail-outer-accordion" open>
    <summary class="test-detail-outer-summary">
      Policy Generator Prompt 
  <br>
  Create tailored policies directly for <b>Control Code: ' || control_id || '</b> &mdash; <b>FII ID: ' || fii_id || '</b>.
  The "Policy Generator Prompt" lets you transform abstract requirements into actionable, 
  written policies. Simply provide the relevant control or framework element, and the prompt
  will guide you in producing a policy that aligns with best practices, regulatory standards, 
  and organizational needs. This makes policy creation faster, consistent, and accessible—even 
  for teams without dedicated compliance writers.
    </summary>
    <div class="test-detail-outer-content">' AS html
FROM compliance_regime_control_hitrust_e1
WHERE control_id = $code::TEXT;

     
    SELECT 'card' as component, 1 as columns;
    SELECT
      '
' || p.body_text AS description_md
      FROM ai_ctxe_complaince_prompt p
      WHERE p.control_id = $code AND p.documentType = 'Author Prompt' and regime = 'HiTRUST'
      ;

    
    SELECT 'html' AS component,
      '</div></details>' AS html;

      --accordion for audit prompt

SELECT 'html' AS component,
  '<details class="test-detail-outer-accordion" open>
    <summary class="test-detail-outer-summary">
      Policy Audit Prompt 
      <br>
      Ensure your policies stay effective and compliant with the "Policy Audit Prompt". These prompts are designed to help users critically evaluate existing policies against standards, frameworks, and internal expectations. By running an audit prompt, you can identify gaps, inconsistencies, or outdated language, and quickly adjust policies to remain audit-ready and regulator-approved. This gives your team a reliable tool for continuous policy improvement and compliance assurance.
    </summary>
    <div class="test-detail-outer-content">' AS html
FROM compliance_regime_control_hitrust_e1
WHERE control_id = $code::TEXT;

    SELECT 'card' as component, 1 as columns;
    SELECT
      '
' || p.body_text AS description_md
      FROM ai_ctxe_complaince_prompt p
      WHERE p.control_id = $code AND p.documentType = 'Audit Prompt' and regime = 'HiTRUST'
      ;
 SELECT 'html' AS component,
      '</div></details>' AS html;

      
SELECT 'html' AS component,
  '<details class="test-detail-outer-accordion" open>
    <summary class="test-detail-outer-summary">
      Generated Policies
      <br>
      The Generated Policies section showcases real examples of policies created using the "Policy Generator Prompt". These samples illustrate how high-level controls are translated into concrete, practical policy documents. Each generated policy highlights structure, clarity, and compliance alignment—making it easier for users to adapt and deploy them within their own organizations. Think of this as a living library of ready-to-use policy templates derived directly from controls.
    </summary>
    <div class="test-detail-outer-content">' AS html
FROM compliance_regime_control_hitrust_e1
WHERE control_id = $code::TEXT;

    SELECT 'card' as component, 1 as columns;
    SELECT
      '
' || p.body_text AS description_md
      FROM ai_ctxe_policy p
      WHERE p.control_id = $code and regimeType = 'HiTRUST';
   SELECT 'html' AS component,
      '</div></details>' AS html;
      SELECT 'html' as component,
    '<style>
        tr.actualClass-passed td.State {
            color: green !important; /* Default to red */
        }
         tr.actualClass-failed td.State {
            color: red !important; /* Default to red */
        }
          tr.actualClass-passed td.Statealign-middle {
            color: green !important; /* Default to red */
        }
          tr.actualClass-failed td.Statealign-middle {
            color: red !important; /* Default to red */
        }
        
        .btn-list {
        display: flex;
        justify-content: flex-end;
        }
       h2.accordion-header button {
        font-weight: 700;
      }

      /* Test Detail Outer Accordion Styles */
      .test-detail-outer-accordion {
        border: 1px solid #ddd;
        border-radius: 8px;
        margin: 20px 0;
        overflow: hidden;
      }

      .test-detail-outer-summary {
        background-color: #f5f5f5;
        padding: 15px 20px;
        cursor: pointer;
        font-weight: 600;
        color: #333;
        border: none;
        outline: none;
        user-select: none;
        list-style: none;
        position: relative;
        transition: background-color 0.2s;
      }

      .test-detail-outer-summary::-webkit-details-marker {
        display: none;
      }

      .test-detail-outer-summary::after {
        content: "+";
        position: absolute;
        right: 20px;
        top: 50%;
        transform: translateY(-50%);
        font-size: 18px;
        font-weight: bold;
        color: #666;
      }

      .test-detail-outer-accordion[open] .test-detail-outer-summary::after {
        content: "−";
      }

      .test-detail-outer-summary:hover {
        background-color: #ebebeb;
      }

      .test-detail-outer-content {
        padding: 20px;
        background-color: white;
        border-top: 1px solid #ddd;
      }
    </style>

    ' as html;


          -- end



 
 
    

    --- Fallback if no exact match
    SELECT 'text' AS component,
          'No exact control found for code: ' || COALESCE($code,'(empty)') AS contents
    WHERE NOT EXISTS (
      SELECT 1 FROM compliance_regime_control_hitrust_e1 WHERE control_id = $code
    );

```

THSA page

```sql ce/regime/thsa.sql { route: { caption: "Together.Health Security Assessment (THSA)" } }
SET resource_json = sqlpage.read_file_as_text('spry.d/auto/resource/${ctx.path}.auto.json');

SELECT 'title' AS component, (SELECT COALESCE(title, caption)
    FROM sqlpage_aide_navigation
   WHERE namespace = 'prime' AND path = 'ce/regime/thsa.sql/index.sql') as contents;
    ;
  
-- Breadcrumbs
SELECT 'breadcrumb' AS component;
  
SELECT
  'Home' AS title,
   '/' AS link;
  
SELECT
  'Together.Health Security Assessment (THSA)' AS title,
  '#' AS link;  
  
-- Page Heading
SELECT
  'text' AS component,
  $page_title AS title;
  
SELECT
  'The THSA controls provide compliance requirements for health services, mapped against the Secure Controls Framework (SCF).' AS contents;
  
-- Pagination controls (top)
SET total_rows = (SELECT COUNT(*) FROM compliance_regime_thsa );
SET limit = COALESCE($limit, 50);
SET offset = COALESCE($offset, 0);
SET total_pages = ($total_rows + $limit - 1) / $limit;
SET current_page = ($offset / $limit) + 1;
  
-- Table
SELECT
  'table' AS component,
  TRUE AS sort,
  TRUE AS search,
  "Control Code" AS markdown;
  
SELECT
  '[' || scf_code || '](' ||
     '/ce/regime/thsa_detail.sql?id=' || scf_code || ')' AS "Control Code",
  scf_domain AS "Domain",
  scf_control AS "Control",
  scf_control_question AS "Control Question"
FROM compliance_regime_thsa
ORDER BY scf_code
LIMIT $limit OFFSET $offset;
  
-- Pagination controls (bottom)
SELECT 'text' AS component,
    (SELECT CASE WHEN CAST($current_page AS INTEGER) > 1 THEN '[Previous](?limit=' || $limit || '&offset=' || ($offset - $limit) || ')' ELSE '' END)
    || ' '
    || '(Page ' || $current_page || ' of ' || $total_pages || ") "
    || (SELECT CASE WHEN CAST($current_page AS INTEGER) < CAST($total_pages AS INTEGER) THEN '[Next](?limit=' || $limit || '&offset=' || ($offset + $limit) || ')' ELSE '' END)
    AS contents_md
;        

```

THSA Detail page

```sql ce/regime/thsa_detail.sql { route: { caption: "Together.Health Security Assessment (THSA) Detail" } }
SET resource_json = sqlpage.read_file_as_text('spry.d/auto/resource/${ctx.path}.auto.json');


    SELECT
      'breadcrumb' AS component;
 
    SELECT
      'Home' AS title,
       '/' AS link;
 
    SELECT
      'Together.Health Security Assessment (THSA)' AS title,
       '/ce/regime/thsa.sql' AS link;
 
    -- Dynamic last breadcrumb using the reference from the DB
    SELECT
      scf_code AS title,
      '#' AS link
    FROM compliance_regime_thsa
    WHERE scf_code = $id::TEXT;
 
    -- Main Control Detail Card
    SELECT
      'card' AS component,
      $page_title AS title,
      1 AS columns;
 
    SELECT
      scf_domain AS title,
      '**Control Code:** ' || scf_code || '  

' ||
      '**Control Question:** ' || scf_control_question || '  

'  AS description_md
    FROM compliance_regime_thsa
    WHERE scf_code = $id::TEXT;
 
    -- TODO Placeholder Card
    SELECT
      'card' AS component,
      1 AS columns;
 
 
    SELECT
      'TODO: Policy Generator Prompt' AS title,
      'Create tailored policies directly from compliance and security controls. The **Policy Generator Prompt** lets you transform abstract requirements into actionable, written policies. Simply provide the relevant control or framework element, and the prompt will guide you in producing a policy that aligns with best practices, regulatory standards, and organizational needs. This makes policy creation faster, consistent, and accessible—even for teams without dedicated compliance writers.' AS description_md
    UNION ALL
    SELECT
      'TODO: Policy Audit Prompt' AS title,
      'Ensure your policies stay effective and compliant with the **Policy Audit Prompt**. These prompts are designed to help users critically evaluate existing policies against standards, frameworks, and internal expectations. By running an audit prompt, you can identify gaps, inconsistencies, or outdated language, and quickly adjust policies to remain audit-ready and regulator-approved. This gives your team a reliable tool for continuous policy improvement and compliance assurance.' AS description_md
    UNION ALL
    SELECT
      'TODO: Generated Policies' AS title,
      'The **Generated Policies** section showcases real examples of policies created using the Policy Generator Prompt. These samples illustrate how high-level controls are translated into concrete, practical policy documents. Each generated policy highlights structure, clarity, and compliance alignment—making it easier for users to adapt and deploy them within their own organizations. Think of this as a living library of ready-to-use policy templates derived directly from controls.' AS description_md;

```

ISO 27001 v3 Control Details page

```sql ce/regime/iso-27001_detail.sql { route: { caption: "ISO 27001 v3 Control Details" } }


    --- Breadcrumbs
    SELECT 'breadcrumb' AS component;
    SELECT 'Home' AS title, sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/' AS link;    
    SELECT 'ISO 27001 v3' AS title, sqlpage.environment_variable('SQLPAGE_SITE_PREFIX') || '/ce/regime/iso-27001.sql' AS link;
    SELECT COALESCE($code, '') AS title, '#' AS link;

    --- Primary details card
    SELECT 'card' AS component, $page_title AS title, 1 AS columns;
    SELECT
        COALESCE(control_code, '(unknown)') AS title,
        '**SCF Domain:** ' || COALESCE(scf_domain,'') || '  

' ||
        '**SCF Control:** ' || COALESCE(scf_control,'') || '  

' ||
        '**Control Description:** ' || COALESCE(control_description,'') || '  

' ||
        '**Control Question:** ' || COALESCE(control_question,'') || '  

' ||
        '**Evidence:** ' || COALESCE(evidence,'') AS description_md
    FROM compliance_iso_27001_control
    WHERE control_code = $code
    LIMIT 1;

    -- TODO Placeholder Card
    SELECT
      'card' AS component,
      1 AS columns;
 
 
    SELECT
      'TODO: Policy Generator Prompt' AS title,
      'Create tailored policies directly from compliance and security controls. The **Policy Generator Prompt** lets you transform abstract requirements into actionable, written policies. Simply provide the relevant control or framework element, and the prompt will guide you in producing a policy that aligns with best practices, regulatory standards, and organizational needs. This makes policy creation faster, consistent, and accessible—even for teams without dedicated compliance writers.' AS description_md
    UNION ALL
    SELECT
      'TODO: Policy Audit Prompt' AS title,
      'Ensure your policies stay effective and compliant with the **Policy Audit Prompt**. These prompts are designed to help users critically evaluate existing policies against standards, frameworks, and internal expectations. By running an audit prompt, you can identify gaps, inconsistencies, or outdated language, and quickly adjust policies to remain audit-ready and regulator-approved. This gives your team a reliable tool for continuous policy improvement and compliance assurance.' AS description_md
    UNION ALL
    SELECT
      'TODO: Generated Policies' AS title,
      'The **Generated Policies** section showcases real examples of policies created using the Policy Generator Prompt. These samples illustrate how high-level controls are translated into concrete, practical policy documents. Each generated policy highlights structure, clarity, and compliance alignment—making it easier for users to adapt and deploy them within their own organizations. Think of this as a living library of ready-to-use policy templates derived directly from controls.' AS description_md;

    --- Fallback if no exact match
    SELECT 'text' AS component,
          'No exact control found for code: ' || COALESCE($code,'(empty)') AS contents
    WHERE NOT EXISTS (
      SELECT 1 FROM compliance_iso_27001_control WHERE control_code = $code
    );

```

## Quick start

1. Ingest CSV files (recursively) and transform them into the SQLPage database (`sqlpage.db`):
   
   ```bash
   surveilr ingest files --csv-transform-auto -r ingest -d sqlpage.db
   ```

2. Build the SQLPage notebook page from `README.md` and pipe into the database:
  
   ```bash
   ../../lib/sqlpage/spry.ts --md README.md --package --conf sqlpage/sqlpage.json | sqlite3 sqlpage.db
   ```

3. Start the SQLPage server:

   - Windows: `set SQLPAGE_SITE_PREFIX="" &&sqlpage.exe`
   - Linux (from repository root): `SQLPAGE_SITE_PREFIX="" sqlpage.bin`
   - macOS (Homebrew): `SQLPAGE_SITE_PREFIX="" sqlpage`

4. Open your browser at the configured port (default in `README.md` example: `http://localhost:9221`).

### Notes
- This folder assumes you have the SQLPage tooling from the repository (see `lib/sqlpage`).
- [`surveilr`](https://www.surveilr.com/) is used to ingest CSV files — ensure it is installed or available in your PATH.
- Commands above assume a Unix-like shell; Windows paths/commands differ slightly.
- The top of this `README.md` contains a YAML front-matter example used by SQLPage:
  - siteName: Sets the site name as Cpation-Explorer.
  - database_url: Points to the SQLite database (sqlpage.db) in read-write-create mode.
  - web_root: Defines the web root directory for serving files (./)
  - allow_exec: Enables execution of scripts/SQLPage commands.
  - port: Configures the web server to run on port 9221.
  - Adjust the `database_url` and `port` as needed.

## Development: auto rebuild & restart

During active development it's convenient to automatically rebuild the packaged page and restart the `sqlpage.bin` server when markdown changes. The following example uses `watchexec` to watch `.md` files, rebuild the notebook with the repository `codebook` tool, write the output into `sqlpage.db`, and restart the local `sqlpage.bin` server:

```sh
watchexec -e md -- bash -c 'pkill -f sqlpage.bin || true; deno run -A ../../lib/sqlpage/spry.ts --md README.md --package --conf sqlpage/sqlpage.json | sqlite3 sqlpage.db; sleep 1; SQLPAGE_SITE_PREFIX="" sqlpage.bin &'
```

### Notes:

- This command assumes `watchexec`, `deno`, and `sqlite3` are installed and available in your PATH.
   - Install `watchexec` from: https://webinstall.dev/watchexec/
- The `pkill` call attempts to stop any running `sqlpage.bin` process before starting a fresh instance. On systems without `pkill`, stop the server manually.
- The one-second `sleep` gives SQLite a moment to flush the write before the server restarts.

## Troubleshooting

- If the server won't start, confirm `sqlpage` binary exists and is executable. On Linux you may need to run `chmod +x sqlpage.bin` from repo root.
- If pages fail to render, check the `sqlpage.db` file for schema.