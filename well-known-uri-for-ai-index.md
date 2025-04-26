# Well-known URI for AI Brand Knowledge Discovery

**URI suffix**: `ai-index`

**Author and change controller**:  
Jonathan Bowker, jonathan@advancedanalytica.co.uk  
(Willing to delegate change control to an IETF or W3C working group if required.)

## Background (non-normative)

Current well-known URI suffixes are designed mostly for technical protocols, security verifications, or proprietary service discovery.  
There is no standard method for organisations to expose **brand, marketing, and identity metadata** to AI systems in a machine-readable, structured, globally recognisable way.

As AI increasingly mediates brand discovery, content generation, and consumer interaction, the **need for brands to control and surface official assets** to AI systems becomes critical.

The Brando Schema introduces the concept of a **structured, brand-first AI index**, enabling brands to voluntarily expose curated knowledge assets in a trusted, machine-readable form.  
This proposal standardises the location and minimal structure of this data via:

- `/.well-known/ai-index.json`

This approach enables:

- **Machine-readable discovery** of brand-approved knowledge by AI crawlers and LLMs
- **Standardisation** of brand asset exposure for public use
- **Brand safety**, ensuring that AI outputs align with officially published brand material

The `ai-index.json` file is the **entry point** into an organisation’s **Brando®-structured knowledge base** for AI systems.

## Terminology

- **PUBLISHER** refers to any organisation publishing an `ai-index.json` file.
- **AI CRAWLER** refers to any agent (crawler, LLM, tool) that fetches and parses `ai-index.json` for brand knowledge discovery.
- **DOMAIN** refers to the root web domain serving the `/.well-known/ai-index.json`.
- Terms **MUST**, **MUST NOT**, **SHOULD**, **RECOMMENDED**, and **MAY** are used as defined in [RFC 2119].

## Usage

To implement the `/.well-known/ai-index.json`:

1. **Publisher** MUST create a `JSON-LD` file located at:

   ```
   https://[yourdomain]/.well-known/ai-index.json
   ```

2. **Publisher** MUST ensure the file has:

   - `@context` field including both `"schema": "https://schema.org/"` and `"brando": "https://brandoschema.com/"`.
   - `@type` set to `"brando:KnowledgeCollection"`.
   - `schema:name`, `schema:description`, and `schema:creator`.
   - `schema:hasPart`, listing **brando:KnowledgeAsset** or **schema:CreativeWork** entries.

3. Each entry in `schema:hasPart` MUST include:

   - `schema:name`
   - `schema:description`
   - `schema:url`

4. **AI Crawlers** MAY treat the presence of an `ai-index.json` file as a signal that **machine-readable brand metadata is available for ingestion**.

5. **All referenced assets** MUST be publicly accessible without authentication or payment barriers.

## Security and Privacy Considerations

- Publishers MUST restrict exposure to **non-confidential**, **approved** brand assets only.
- Materials inappropriate for general public AI consumption MUST NOT be linked.
- HTTPS MUST be used to protect the integrity of AI-index files during transmission.

## Example `/.well-known/ai-index.json`

```json
{
  "@context": {
    "schema": "https://schema.org/",
    "brando": "https://brandoschema.com/"
  },
  "@type": "brando:KnowledgeCollection",
  "schema:name": "Acme Corporation AI Knowledge Index",
  "schema:description": "Machine-readable index of Acme Corporation's official brand knowledge for AI applications.",
  "schema:creator": {
    "@type": "schema:Organization",
    "schema:name": "Acme Corporation",
    "schema:url": "https://www.acme.com/"
  },
  "schema:hasPart": [
    {
      "@type": "brando:KnowledgeAsset",
      "schema:name": "Verbal Identity Guide",
      "schema:description": "Guidelines for Acme's brand tone, language, and writing style.",
      "schema:url": "https://www.acme.com/ai-assets/verbal-identity-guide.json"
    },
    {
      "@type": "brando:KnowledgeAsset",
      "schema:name": "Logo Usage Guidelines",
      "schema:description": "Rules and specifications for correct Acme logo usage.",
      "schema:url": "https://www.acme.com/ai-assets/logo-usage-guide.json"
    }
  ]
}
```

## Appendix (non-normative)

**Primary Goals**:

- Empower brands to define their AI-facing presence proactively.
- Reduce misinformation and misuse of brand assets by AI systems.
- Create a **voluntary**, **decentralised** mechanism for structured brand data exposure.

**Non-goals**:

- Force indexing of all brand assets.
- Mandate crawler behavior beyond retrieval and parsing.
- Provide certification or verification of publisher identity.

## References

- [RFC8615]  M. Nottingham, "Well-Known Uniform Resource Identifiers (URIs)", RFC 8615, May 2019.
- [RFC2119]  S. Bradner, "Key words for use in RFCs to Indicate Requirement Levels", RFC 2119.
- [Schema.org] "Schema.org Core Vocabulary", https://schema.org/
- [Brando Schema] "Brando Brand Definition Vocabulary", https://brandoschema.com/
