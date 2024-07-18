# Nighthawk's Rules Collection for Rule Engine v0.1

## Introduction

Welcome to the Nighthawk Rules Collection! This repository is dedicated to storing rules in JSON format contributed by users like you. These rules can be used with the Nighthawk Rule Engine to enhance the detection and prevention of phishing attacks.

## How to Contribute

We encourage you to contribute your own rules to this collection. To do so, please follow these steps:

1. Fork this repository to your own GitHub account.
2. Create a new branch for your rule.
3. Write your rule in JSON format, following the guidelines provided below.
4. Commit and push your changes to your forked repository.
5. Open a pull request to submit your rule for review.

## Rule Format

Each rule should be written in JSON format and include the following properties:

- `name`: A descriptive name for the rule.
- `description`: A brief description of what the rule is intended to detect.
- `author`: The name of the rule author
- `target`: Targeted brand the scam is targeting
- `composite_flag_condition`: The conditions that must be met for the rule to trigger.
- `composite_false_positive`: The condition that must be met for the engine to skip the rule

Types of the fields

```ts
enum EOperator {
  CONTAINS = "contains",
  NOT_CONTAINS = "not_contains",
  NOT_EQUALS = "not_equals",
  LENGTH_GREATER_THAN = "length_greater_than",
  LENGTH_LESS_THAN = "length_less_than",
  STARTS_WITH = "starts_with",
  ENDS_WITH = "ends_with",
  EQUAL_TO_ANY = "equalToAny",
}

interface ICompositeCondition {
  match_condition: EMatchCondition;
  conditions: ICompositeFlagCondition[];
}

interface ICompositeFlagCondition {
  logical_operator: ELogicalOperator;
  conditions: ICondition[];
}

interface ICondition {
  attribute: string;
  operator: EOperator;
  value: string;
  case_sensitive?: boolean;
}
```

Here's an example of a rule in JSON format:

```json
{
  "name": "walken-nft-web",
  "author": "Daniel Demelash",
  "description": "Rule for scam site that claim to give free Walken mint, similar to walken-nft.web.app",
  "target": "Walken NFT Games",
  "composite_flag_conditions": {
    "match_condition": "any",
    "conditions": [
      {
        "logical_operator": "and",
        "conditions": [
          {
            "attribute": "html",
            "operator": "contains",
            "value": "Mint Now"
          },
          {
            "attribute": "html",
            "operator": "contains",
            "value": "Mint is Live!",
            "case_sensitive": true
          },
          {
            "attribute": "html",
            "operator": "contains",
            "value": "hurry!"
          },
          {
            "attribute": "title",
            "operator": "contains",
            "value": "Walken Whitelist",
            "case_sensitive": true
          }
        ]
      },
      
    ]
  },
  "composite_false_positives": {
    "match_condition": "any",
    "conditions": [
      {
        "logical_operator": "or",
        "conditions": [
          {
            "attribute": "url",
            "operator": "contains",
            "value": "walken.io"
          }
        ]
      }
    ]
  }
}
```

Thank you for your contribution!
