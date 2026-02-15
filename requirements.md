# Vyapar-IQ — Requirements Document
## "The AI Profit-Guard for Kiranas"
### Hack to Skill Hackathon | Powered by AWS | AI for Bharat

---

## Table of Contents

1. Executive Summary
2. Problem Statement
3. Solution Overview
4. User Personas
5. Functional Requirements
6. Non-Functional Requirements
7. Business Model and Revenue
8. Competitive Analysis
9. Why AI is Needed — Not Rule-Based Logic
10. AWS Services Mapping
11. Success Metrics and KPIs
12. Risks and Mitigations
13. Ethical AI and Responsible Design
14. Glossary

---

## 1. Executive Summary

Vyapar-IQ is an AI-powered B2B retail intelligence platform that acts as a "Chief Merchandising Officer" for India's 12+ million Kirana (small retail) stores. It uses AWS Bedrock's multimodal AI to solve three existential problems for small retailers: Dead Capital (money stuck in unsold/expired products), Thin Margins (8-12% from big brands with middlemen eating the value chain), and Zero Market Intelligence (no data on what to stock, when to stock, or how to price).

The platform deploys four AI agents working in concert. The Shelf Analyst uses visual AI to track inventory velocity from shelf photos. The Trend Hunter predicts demand spikes from weather, events, and festivals. The Liquidation Bot prevents spoilage through dynamic pricing and auto-generated customer discount campaigns. The Trade-Promo Agent directly connects Kirana stores with small and emerging brands, eliminating middlemen and boosting margins from 8% to 25-40%.

The platform also features a Brand Web Dashboard where small brands can post bounties, run sampling campaigns, discover high-performing retailers, and access aggregated market intelligence. Logistics are handled through ONDC (Open Network for Digital Commerce), creating an end-to-end ecosystem from brand discovery to product delivery.

Target Impact: Convert ₹5,000 to ₹15,000 of monthly losses per store into profit, while giving small brands direct access to India's largest retail network.

---

## 2. Problem Statement

### 2.1 The Macro Problem

India has over 12 million Kirana stores that account for approximately 80% of India's total retail market worth around \$600 billion. Despite their dominance, these stores operate on razor-thin margins of 8-12% from established FMCG brands. They lose an estimated 5-8% of total stock value monthly to spoilage of perishables like milk, bread, and vegetables. They have zero access to demand forecasting tools, leading to chronic overstocking of slow items and understocking of high-demand items. They are completely dependent on middlemen such as distributors and wholesalers who take 15-30% of the product's value chain, leaving the retailer with minimal margin. They have no visibility into emerging brands that could offer them 25-40% margins instead of the usual 8%.

### 2.2 The Micro Problem — A Day in the Life

Consider Ramesh, who owns a Kirana store in Jayanagar, Bangalore.

On Monday at 6 AM, he orders 50 packets of bread. By Wednesday, 15 are unsold and expired. He loses ₹450. On Wednesday at 3 PM, India vs Pakistan cricket match is announced for Friday. He doesn't know, so he doesn't stock extra chips and cold drinks. His competitor across the street does. He loses ₹2,000 in potential sales. On Thursday at 10 AM, a new local masala brand would happily give him 30% margins to push their product, but they have no way to reach Ramesh and Ramesh has no way to discover them. He continues selling the big brand masala at 8% margin. On Friday at 7 PM, he sells rice by the kilogram, loose and unbranded. A packaged rice brand would pay him ₹10 per kg margin versus his current ₹5 per kg if he just stocked their brand. But neither Ramesh nor the brand knows about each other.

Monthly financial impact on a typical Kirana store:
- Spoilage from expired bread, milk, and vegetables: ₹3,000 to ₹5,000
- Missed sales from events, weather, and festivals: ₹2,000 to ₹4,000
- Lost potential from low margins with big brands only: ₹5,000 to ₹10,000
- Total preventable loss: ₹10,000 to ₹19,000 per month

On a monthly net profit of ₹20,000 to ₹30,000, this represents a 33-63% profit erosion.

### 2.3 Why Current Solutions Fail

Khatabook and OkCredit are digital ledgers for tracking customer credit. They do not help with inventory, demand forecasting, or margin optimization. They are passive record-keepers.

Udaan and Jumbotail are B2B wholesale ordering apps. They focus on big brands, don't provide AI intelligence, don't connect with small brands, and don't forecast demand.

Tally and Busy are accounting software products. They are too complex for most Kirana owners, require extensive data entry, and have no AI capabilities.

Manual intuition, the shopkeeper's gut feeling, fails when external factors like weather, events, new trends, and seasonal shifts change rapidly. Human intuition cannot process multiple data signals simultaneously.

### 2.4 Why Rule-Based Logic Fails

Consumer demand depends on the intersection of weather, local events like cricket matches and school exam seasons, festivals like Diwali and Eid and Pongal, neighborhood demographics, day of the week, time of the month relative to payday, and social media trends. No finite set of IF/THEN rules can capture this complexity.

Visual inventory analysis requires perception. Counting items on a shelf from a photograph, identifying brand logos, reading expiry dates, and assessing shelf placement quality require computer vision and multimodal AI reasoning, not simple conditionals.

Brand-retailer matchmaking requires semantic understanding. Matching a retailer who sells 200kg of loose rice per week with an emerging packaged rice brand requires the AI to understand product categories, consumer behavior patterns, regional preferences, and margin economics.

Natural language interaction is essential. The target user has low digital literacy and needs to speak in Hindi, Telugu, Tamil, or their regional language with the AI understanding context, intent, and nuance.

---

## 3. Solution Overview

### 3.1 What is Vyapar-IQ?

Vyapar-IQ is a multimodal AI platform with two interfaces. The first is a Retailer Mobile App for Android, designed for Kirana store owners, supporting voice commands, text input, and camera-based shelf scanning, delivering AI insights via push notifications and in-app Deal Cards. The second is a Brand Web Dashboard for small and emerging FMCG brands, allowing them to post bounties, manage sampling campaigns, view aggregated market intelligence, and track retailer performance.

These two interfaces are connected by a central AI Reasoning Engine powered by AWS Bedrock that runs four specialized agents.

### 3.2 The Four AI Agents

Agent 1 is the Shelf Analyst, responsible for visual intelligence. It processes shelf photos to count inventory, detect dead stock, read expiry dates, calculate sales velocity by comparing photos over time, and verify brand placement for Trade-Promo contracts.

Agent 2 is the Trend Hunter, responsible for hyperlocal intelligence. It continuously scans weather forecasts, sports events, local news, festival calendars, and the store's own historical data to predict demand spikes and dips 24-48 hours in advance.

Agent 3 is the Liquidation Bot, responsible for dynamic pricing and spoilage prevention. It identifies items approaching expiry, calculates optimal discount strategies, generates localized WhatsApp discount messages, and tracks outcomes to improve future recommendations.

Agent 4 is the Trade-Promo Agent, responsible for brand collaboration and margin boosting. It acts as a B2B marketplace connecting Kirana stores directly with small and emerging brands, enabling trade promotions, sampling campaigns, and converting unbranded volume into branded volume.

### 3.3 Core Philosophy

We are not building an inventory manager. We are building an AI system that rotates capital. It converts dead stock into cash before it expires. It predicts future sales so the shopkeeper never misses a customer. It discovers hidden margin opportunities by connecting retailers directly with emerging brands. It eliminates middlemen in the supply chain, passing savings to both the brand and the retailer. It converts unbranded volume into branded volume, which is the holy grail of Indian retail.

---

## 4. User Personas

### Persona 1: Ramesh — The Kirana Owner

Ramesh is aged 35-55, located in a Tier 2 or Tier 3 city or semi-urban India. His tech literacy is low to medium — he uses WhatsApp and YouTube but is uncomfortable with complex apps. He uses a budget Android phone in the ₹8,000 to ₹15,000 range. He speaks Hindi and his regional language with limited English. His monthly revenue is ₹2,00,000 to ₹5,00,000 with a net profit of ₹20,000 to ₹40,000. His pain points are spoilage losses, low margins, no demand visibility, and being too busy to learn new software. He is motivated by more profit with less effort and gaining a competitive edge over neighboring stores. His current tools are a paper notebook, basic calculator, and WhatsApp for customer communication. He prefers voice commands in local language, photos, and minimal typing.

### Persona 2: Priya — The Small Brand Owner

Priya is aged 28-45 and can be located in any city in India. Her tech literacy is medium to high and she is comfortable with web dashboards. She is the founder or marketing head of a small or emerging FMCG brand selling products like packaged rice, local masala, organic snacks, or regional beverages. Her monthly marketing budget ranges from ₹50,000 to ₹5,00,000. Her pain points are that she cannot reach Kirana stores, distributors demand 15-30% cuts, she has no visibility into actual shelf placement, and she has no data on which locations have demand. She is motivated by direct access to retailers, proof of shelf placement, and data on where her products sell best. She prefers a web dashboard with analytics and campaign management.

### Persona 3: The Distributor (Reduced Role)

Currently a middleman between brands and retailers, the distributor's role is significantly reduced by Vyapar-IQ for small brands. However, for logistics, they may still participate through the ONDC network. The platform does not eliminate distributors entirely. For brands that already have distribution, Vyapar-IQ complements existing channels. For small brands that have no distribution, it provides direct access plus ONDC logistics.

---

## 5. Functional Requirements

### 5.1 Agent 1: Shelf Analyst — Visual Intelligence

#### Purpose
Eliminate manual stock counting by using AI visual analysis to track inventory levels, identify dead stock, assess product velocity, verify brand placement for Trade-Promo contracts, and read expiry dates.

#### User Stories

SA-01 (Must Have): As a Kirana owner, I want to take a photo of my shelf and have the AI count how many items of each product I have, so I don't have to manually count.

SA-02 (Must Have): As a Kirana owner, I want the AI to compare today's shelf photo with yesterday's photo and tell me which items are selling fast (high velocity) and which are sitting idle (dead stock), so I know what to reorder and what to stop buying.

SA-03 (Must Have): As a Kirana owner, I want the AI to read expiry dates from product packaging in the photo and alert me about items expiring within 1-3 days, so I can take action before they spoil.

SA-04 (Must Have): As a Kirana owner, I want the AI to tell me something like "You have 20 packets of Brand X chips sitting here since last week's photo. This is dead stock. Stop buying this," so I stop wasting money.

SA-05 (Should Have): As a Brand via Trade-Promo, I want the AI to verify from shelf photos that the retailer has placed my product prominently at eye-level and front of shelf, so I can confirm my visibility agreement is being honored.

SA-06 (Should Have): As a Kirana owner, I want to scan multiple shelves and sections of my store such as Snacks, Dairy, Beverages, and Staples and get a consolidated inventory report.

SA-07 (Should Have): As a Kirana owner, I want the AI to identify items in the photo even if they are partially hidden or at angles.

#### AI Behavior Specification

The input is a JPEG or PNG image from the phone's rear camera.

The AI processing through AWS Bedrock Claude Sonnet Multimodal performs the following steps. First, Object Detection and Counting where it identifies distinct product SKUs in the image, counts the quantity of each, and handles stacked items, partially occluded items, and varied lighting conditions. Second, Brand and Product Recognition where it identifies brand names, product names, and variants from packaging labels, distinguishing for example between "Lay's Classic Salted 50g" and "Lay's Magic Masala 50g." Third, Expiry Date Extraction through OCR where it reads printed expiry dates on visible packaging. Fourth, Velocity Calculation through Comparative Analysis where it compares the current photo with the most recent previous photo of the same shelf section. Items present in the previous photo but absent now are classified as Sold Items. Items present in both photos with the same quantity are classified as Slow or Dead Stock. New items not in the previous photo are classified as New Stock Added. Fifth, Shelf Placement Analysis for Trade-Promo where it assesses the placement quality of a specific brand's product, checking whether it is at eye level, at the front of the shelf, and whether the brand label is facing forward, then outputting a Placement Score from 1 to 10 with reasoning.

The output is a structured analysis plus a Natural Language Summary in the user's preferred language.

#### Edge Cases and AI Handling

If the photo is blurry or dark, the AI responds asking the user to take the photo again with better lighting. It does NOT guess.

If the shelf is empty, the AI reports that and offers to suggest what to stock based on the area's demand.

If products have labels not visible because they face backward, the AI reports it can see items but cannot read labels and asks the user for a closer photo or verbal confirmation.

For the same product in different sizes, the AI distinguishes based on visual size difference and packaging text. If ambiguous, it asks the user.

For non-packaged items like loose rice or dal in bins, the AI estimates quantity visually with an approximation like "approximately 5kg of loose rice remaining" and asks for confirmation.

For the first-time scan with no previous photo to compare, the AI establishes a baseline, reports current stock only, and instructs the user to take another photo tomorrow so it can calculate what's selling.

#### Offline Behavior

The shopkeeper can take photos offline. Photos are queued locally with timestamps. When connectivity returns, photos auto-upload for processing. AI analysis results are delivered as soon as processing completes.

---

### 5.2 Agent 2: Trend Hunter — Hyperlocal Intelligence

#### Purpose
Predict short-term demand spikes and dips based on external factors that the shopkeeper cannot track manually. Act as a proactive early warning system for inventory decisions.

#### User Stories

TH-01 (Must Have): As a Kirana owner, I want the AI to alert me 24-48 hours before a demand spike caused by a cricket match, rain, or festival so I can stock up in advance and not miss sales.

TH-02 (Must Have): As a Kirana owner, I want the AI to tell me WHAT to stock extra with specific products, not just that demand will increase, so I can take immediate action.

TH-03 (Should Have): As a Kirana owner, I want the AI to also warn me about demand DIPS such as school exams next week meaning ice cream sales will drop 40%, so I don't overorder.

TH-04 (Nice to Have): As a Kirana owner, I want the AI to learn over time that my specific store has different patterns from other stores, for example that my area has more families so Maggi sells less during school holidays because kids eat home food.

#### Data Sources

The Trend Hunter scans multiple external data signals. Weather forecasts come from OpenWeatherMap API or equivalent, updated every 6 hours. Sports events come from public sports calendars and news RSS feeds, checked daily. The festival calendar is a pre-loaded Indian festival database organized by region and religion, supplemented by AI reasoning. Local news comes from News API or Google News RSS for the specific city or district, checked every 12 hours. The store's own historical velocity data comes from the Shelf Analyst, updated continuously. Neighborhood demographics are inferred by the AI from the store's product ordering patterns, updated monthly.

#### AI Reasoning Process

The AI does not use hardcoded rules. It performs multi-factor reasoning. For example, given that the location is Jayanagar Bangalore on a Friday in February with heavy rain forecast for Saturday and an India vs Pakistan cricket match on Saturday at 7 PM, and that the store is in a family neighborhood with high weekend footfall, and that during the last cricket match day this store sold 3x more chips and 2x more cold drinks, the AI reasons as follows. Rain plus cricket match means people will stay home and watch the match. Staying home plus watching cricket means snack consumption spikes. Family neighborhood means also expect tea and pakora ingredients like besan and onions. Based on all these factors, the AI generates a specific alert with product recommendations and estimated additional revenue.

#### Alert Types

Demand Spike Alerts are triggered when events, weather, and historical data suggest more than 50% increase. The output is something like "Stock up on these items, demand will spike X%."

Demand Dip Warnings are triggered when factors suggest more than 30% decrease. The output is something like "Don't overorder these items this week, demand will drop."

Seasonal Shift Alerts are triggered by calendar and weather pattern changes. The output is something like "Summer is starting, begin stocking cold drinks, ice cream, and ORS."

Opportunity Alerts are triggered by unique local events. The output is something like "IPL team practice session happening near your area, stock energy drinks and snacks."

#### Edge Cases

When no significant events are detected, the AI stays silent. It does NOT send alerts just to seem active. Silence means everything is normal.

When there are conflicting signals, the AI explains the conflict, for example "Rain suggests low footfall but Diwali suggests high demand. Net recommendation: Stock up but 20% less than usual Diwali."

When the AI is unsure, it says so explicitly, for example "I'm not confident about this prediction. A local political rally might affect your area. Keep an eye out and tell me if your street is blocked."

When a store has no history yet, the AI uses city-level averages and explicitly states "This is based on stores similar to yours in Bangalore. After 2-3 weeks, I'll learn your specific patterns."

---

### 5.3 Agent 3: Liquidation Bot — Dynamic Pricing and Spoilage Prevention

#### Purpose
Prevent financial losses from expired products by proactively suggesting dynamic pricing, discount offers, and customer outreach campaigns BEFORE items expire.

#### User Stories

LB-01 (Must Have): As a Kirana owner, I want the AI to alert me 2-3 days before perishable items expire so I have time to act.

LB-02 (Must Have): As a Kirana owner, I want the AI to automatically suggest a specific discount percentage that maximizes my revenue recovery, explaining for example "Sell bread at Buy 1 Get 1 Free, you recover ₹300 instead of losing ₹450."

LB-03 (Must Have): As a Kirana owner, I want the AI to draft a WhatsApp message with the discount offer that I can forward to my customer group with one tap.

LB-04 (Should Have): As a Kirana owner, I want the AI to suggest WHO to send the offer to first, such as customers who usually buy bread, so the offer reaches the most relevant people.

LB-05 (Nice to Have): As a Kirana owner, I want the AI to learn from past liquidation results and improve its suggestions.

#### How the AI Knows Expiry Dates

The AI uses multiple methods and NEVER guesses. The primary method is OCR from Shelf Photos where the Shelf Analyst reads expiry dates printed on packaging during shelf scans. The second method is Voice Input from the Shopkeeper where they say something like "I bought 50 milk packets today, expiry 16th February" and the AI logs it. The third method is Text Input where the shopkeeper types or selects the product, quantity, and expiry date. The fourth method is Product-Type Default Estimation, but ONLY as a fallback and the AI ALWAYS asks for confirmation. If no expiry date is available and the shopkeeper doesn't provide one, the AI says "I don't know the expiry date for this milk. Milk usually lasts 5 days. When did you buy it?" It asks, it does NOT assume. The fifth method is a Brand Database that over time records typical shelf lives, used only to prompt the shopkeeper for confirmation.

Critical Rule: The AI NEVER guesses an expiry date silently. If it doesn't know, it asks.

#### Pricing Optimization Logic

The AI models multiple discount scenarios for each expiring product, considering the cost price, selling price, current stock, normal daily sales velocity, and days until expiry. It calculates how many will sell at normal price, how many will be wasted, what the total loss would be with no action, and then models various discount levels to find the one that maximizes revenue recovery. It factors in the additional demand boost from a WhatsApp broadcast. The AI then recommends the optimal scenario with a clear explanation of the financial outcome.

#### WhatsApp Message Generation

When the shopkeeper approves, the AI generates a professional, localized message in the shopkeeper's language with the product name, original price versus offer price, urgency messaging, store name and location, and appropriate emoji for visual appeal. The message is pre-filled and the shopkeeper taps one button to open WhatsApp and forward it to their customer groups.

#### Customer Data Sources

Most Kirana owners already have WhatsApp groups of regular customers. The AI simply drafts the message and the shopkeeper forwards it. No new customer data collection is needed. Over time, if the shopkeeper uses a UPI QR code, the app can with consent note which customers buy which products, enabling targeted messaging. The shopkeeper can also manually voice-tag customers like "Mrs. Sharma buys bread every Monday" and the AI remembers.

---

### 5.4 Agent 4: Trade-Promo Agent — Brand Collaboration and Margin Booster

#### Purpose

This is the core differentiator of Vyapar-IQ. It directly increases the retailer's profit margins by connecting them with small and emerging brands that offer 25-40% margins versus 8-12% from big brands, by eliminating middlemen so both brand and retailer get a better deal, by enabling brands to pay for Trade Promotions giving retailers extra margins for prominent display, and by converting unbranded volume like loose rice, dal, and jaggery into branded volume by finding packaged alternatives.

#### User Stories — Retailer Side

TP-01 (Must Have): As a Kirana owner, I want the AI to show me deals from brands that match my store's product categories so I can discover new high-margin products.

TP-02 (Must Have): As a Kirana owner, I want to swipe right on a deal to accept it and have the product delivered to my store via ONDC logistics so I don't have to visit a wholesaler.

TP-03 (Must Have): As a Kirana owner, I want the AI to proactively tell me something like "You sell 200kg of loose rice. Brand Y will give you ₹10/kg margin if you sell their packaged rice instead."

TP-04 (Should Have): As a Kirana owner, I want to earn extra margin by placing a brand's product at the front of my shelf and verifying it with a photo.

TP-05 (Should Have): As a Kirana owner, I want to be able to pass on some of my higher margin as a discount to customers, attracting more footfall.

TP-06 (Should Have): As a Kirana owner, I want to contact brand representatives directly through the platform for negotiation.

#### User Stories — Brand Side

TP-07 (Must Have): As a small brand, I want to post a Bounty (for example "25% margin for any retailer who stocks our new masala") and have it matched to relevant retailers automatically.

TP-08 (Must Have): As a small brand, I want to see which retailers accepted my deal and get photo verification that my product is displayed prominently.

TP-09 (Should Have): As a small brand, I want to see aggregated demand data such as "Retailers in North Bangalore sell 500kg of loose rice per week — opportunity for your packaged rice."

TP-10 (Should Have): As a small brand, I want to run a sampling campaign and track if it leads to increased sales over 14 days.

TP-11 (Should Have): As a small brand, I want to see each retailer's Power Score so I can prioritize who gets my best deals.

#### Deal Types

Standard Margin Deal: Brand offers product at a wholesale price with a specific margin percentage to the retailer. For example, "Fried Rice Masala at ₹30 to retailer, MRP ₹50, Margin 40%."

Trade Promotion or Visibility Deal: Brand pays extra margin IF the retailer displays the product prominently, verified by AI shelf photo analysis. For example, "Extra 5% margin if our masala is placed at eye-level, front shelf."

Sampling Campaign: Brand provides free sample packets and the retailer distributes them to customers with purchases. For example, "50 free mini-packets of our new coffee, hand one to every customer who buys tea."

Volume Deal: Better margin at higher volumes. For example, "Order 100+ packets and get 35% margin instead of 25%."

Unbranded-to-Branded Conversion: AI identifies retailers selling loose goods and matches them with packaged alternatives. For example, "You sell loose jaggery. Brand Z offers organic jaggery at ₹12/kg margin."

#### Middleman Elimination — How Direct Sourcing Works

In the traditional supply chain without Vyapar-IQ, the product goes from Brand to C&F Agent (8% cut) to Distributor (10% cut) to Wholesaler (7% cut) to Retailer (8% margin) to Customer. The total markup from brand to customer is approximately 33% and the retailer gets only 8%.

In the new supply chain with Vyapar-IQ, the product goes from Brand directly via ONDC Logistics to Retailer (25-30% margin) to Customer. The total markup from brand to customer is roughly 30% (about the same for the customer) but the retailer gets 25-30%.

The key insight: If the retailer was selling rice at ₹50/kg with ₹5 margin, now with direct sourcing they can sell at ₹46-47/kg with ₹10+ margin. The customer gets a lower price, the retailer gets a higher margin, and the brand gets direct access to the retail network. Everyone wins.

#### The Default Dispensing Feature

When a customer walks in and says "Give me fried rice masala" without specifying a brand, the current behavior is that the retailer grabs whatever is closest. With Vyapar-IQ, if the retailer has a collaboration with a specific masala brand offering 30% margin, the AI has reminded them to push this brand's product when customers don't specify a preference. The retailer naturally hands over the collaborated brand's packet. The customer gets a good product, the retailer gets 30% margin instead of 8%, and the brand gets a sale. This is standard Trade Promotion practiced by FMCG globally, now powered by AI for small retailers.

#### Payment Between Brand and Retailer

Vyapar-IQ does NOT process payments. The platform acts as an intelligent matchmaker and logistics trigger. Payment is left flexible between the brand and retailer — Cash on Delivery, bank transfer via UPI or NEFT, third-party credit, or any method they mutually agree upon. This avoids regulatory complexity and keeps the platform lightweight.

---

### 5.5 Brand Dashboard — B2B Web Portal

#### Purpose
Give small and emerging brands a self-service portal to access India's Kirana network through Vyapar-IQ.

#### Module 1: Market Intelligence

This module provides a Demand Heatmap showing an aggregated, anonymized view of product demand by geography. For example, "Loose rice consumption is 500kg per week in Jayanagar, Bangalore across 50 stores." It shows Category Trends like "Demand for organic products is up 23% in Tier 2 cities this month." It provides Gap Analysis such as "45 stores in Indiranagar sell loose dal but NO branded dal. Opportunity for your brand." And it shows Seasonal Forecasts like "Summer is approaching. Cold beverage demand will spike 150% in the next 6 weeks."

#### Module 2: Bounty Management (Trade Promotions)

Brands can Create Bounties by setting the product, margin offered, target geography, target store demographics, and duration. They see Match Results showing which retailers were matched by the AI and who accepted or declined. Visual Verification shows shelf photos submitted by retailers confirming product placement with AI-generated placement scores. Performance Tracking shows reorder rates from each retailer.

#### Module 3: Sampling Campaigns

Brands can Create Campaigns specifying the product, sample size, number of samples per store, target geography, and duration. Distribution Tracking shows which stores received samples. Conversion Analysis uses AI to determine if sampling led to increased reorders over 14 days across the cohort. Cohort Intelligence identifies whether poor results are due to product quality issues or individual store non-compliance.

#### Module 4: Retailer Discovery and Power Scores

This shows anonymized Retailer Profiles with categories, volume, and location. Power Scores are AI-generated scores based on sales velocity, shelf placement quality, reorder frequency, and category strength. Category-Specific Scores show, for example, "This store is strong in Cooking/Staples (9/10) but weak in Beverages (3/10)." Demographic Profiles are AI-inferred labels like "Family Area" or "Student/Bachelor Area" or "Premium Locality" based on ordering patterns.

#### Module 5: Analytics and Reports

Campaign ROI Calculator shows total spend versus total retail sales generated. Geographic Performance shows where products perform best and worst. Retailer Cohort Performance shows how different groups of retailers perform. Conversion Funnel Analysis tracks the path from Bounty to Match to Accept to Reorder. All reports are exportable.

#### Data Privacy for Brand Dashboard

Critical Rule: The brand dashboard shows ONLY aggregated, anonymized data. It is acceptable to show "A store in Jayanagar moves 200kg rice per week." It is NOT acceptable to show "Ramesh's store at 4th Cross, Jayanagar moves 200kg rice per week." Retailer identity is revealed ONLY when the retailer explicitly accepts a deal from that brand or opts in to share their store details.

---

### 5.6 Smart Sampling Engine

#### Purpose
Enable brands to distribute free product samples through Kirana stores and use AI to measure effectiveness.

#### Flow

The brand creates a Sampling Campaign on the dashboard specifying the product, quantity per store, target number of stores, and distribution instructions. The AI matches with relevant stores by filtering for geography, category match, high footfall, and good Power Score. Samples are delivered via ONDC logistics. The AI then tracks over 14 days whether stores order the paid version of the product. Results are analyzed at the cohort level.

#### The Quality Catch — Critical Design Decision

Problem identified: If the sample product tastes bad, the customer won't buy it, and it's NOT the shopkeeper's fault. The system must NOT punish the shopkeeper.

Solution using Cohort-Based Analysis: The AI NEVER judges a single store in isolation for sampling effectiveness. It always analyzes the entire cohort. If 0 out of 100 stores reorder, the AI identifies a product quality issue and notifies the brand honestly without penalizing any retailer. If 5 out of 100 reorder, it's still likely a product quality issue. If 80 out of 100 reorder and 20 don't, the AI applies a slight soft adjustment to the non-converting stores' Sampling Trust Score but treats it as a signal, not a punishment. If 95 out of 100 reorder, those 5 non-converting stores receive a small Trust Score reduction while high-converting stores get priority for future campaigns.

The 90% Confidence Rule: The AI only adjusts trust scores when the cohort data gives it 90% or greater statistical confidence that the issue is with the store, not the product.

---

### 5.7 Proactive Brand Discovery Engine

#### Purpose
The AI doesn't just wait for brands to post bounties. It proactively identifies opportunities by analyzing retailer data and searching for matching brands.

#### How It Works

The AI observes from Shelf Analyst data that a store orders 200kg loose rice every week, 50kg loose jaggery every month, and is in a Family Area based on demographic profiling. It searches the platform's brand database for active brands in matching categories targeting the matching geography. If a match is found, it generates a proactive deal card for the retailer and simultaneously notifies the brand.

If no match is found in the platform database, the AI generates an Opportunity Alert on the brand dashboard: "New market opportunity detected: 15 stores in Jayanagar sell 3,000kg loose rice per week. No packaged rice brand currently on platform serving this area."

#### Cross-Category Intelligence for Negotiations

If a retailer collaborates with a masala brand and sells well, this data can be used to negotiate with OTHER brands in DIFFERENT categories. The AI generates a Negotiation Brief showing the store's category performance, customer profile, and recommendations for new brand partnerships.

Trust Rule: Items recommended are from different categories to prevent competitive conflict. It's acceptable to recommend Masala from Brand A, Rice from Brand B, and Coffee from Brand C. It would NOT be acceptable to recommend Masala from Brand A AND Masala from Brand B as that creates conflict.

---

### 5.8 Retailer Power Score System

#### Purpose
Create a trustworthy, AI-generated reputation score for retailers that brands can use to make decisions.

#### Score Components

Sales Velocity carries 30% weight and is measured by how fast inventory depletes, calculated via reorder frequency and shelf photo comparison.

Shelf Compliance carries 20% weight and is measured by whether the retailer maintains good shelf placement for collaborated brands, verified through AI visual analysis.

Reorder Consistency carries 20% weight and is measured by whether the retailer reliably reorders products that sell well and whether the interval is consistent.

Category Strength carries 15% weight and represents per-category velocity scores. A store can be strong in Snacks but weak in Dairy.

Sampling Trust carries 15% weight and measures whether the retailer distributes samples properly when given, adjusted for cohort-level product quality signals.

#### Badges and Benefits

Platinum (Score 9.0+): First access to all new product launches plus highest margin offers.
Gold (Score 7.5+): Priority for sampling campaigns plus featured in brand discovery.
Silver (Score 6.0+): Standard deal access.
Bronze/Growing (Score below 6.0): Standard deal access plus AI coaching tips to improve.

---

### 5.9 ONDC Logistics Integration

#### Purpose
When a deal is struck between a brand and a retailer on Vyapar-IQ, use ONDC (Open Network for Digital Commerce) to handle last-mile delivery.

#### Flow

The retailer accepts a deal by swiping right on a brand's offer. Vyapar-IQ generates an ONDC-compatible order with pickup location at the brand's warehouse, drop location at the retailer's store, items and quantities, and payment method. The ONDC network matches with the nearest logistics provider. The delivery partner picks up and delivers. Vyapar-IQ receives delivery confirmation. The AI logs the delivery for inventory tracking and activates Trade-Promo monitoring.

#### Why ONDC

ONDC is government-backed and recognized by judges. It's an open network where any logistics provider can fulfill orders with no vendor lock-in. It's designed specifically for India's retail infrastructure and works in Tier 1, 2, and 3 cities. It has standard APIs for integration.

For the hackathon demo, ONDC integration can be shown as a simulated API call with mock responses, with the architecture diagram showing the ONDC connection point and the app UI showing delivery status cards.

---

### 5.10 Multi-Modal Input System — Voice Plus Text Plus Camera

#### Purpose
The Kirana owner interacts with Vyapar-IQ through three input modes and chooses their preferred mode at any time.

Camera (Visual Input) is used for shelf scanning, expiry checking, Trade-Promo verification, and logging new stock arrivals.

Voice (Speech Input) is used for stock logging like "I bought 100 eggs today," expiry input like "Milk expiry is February 16," queries like "What should I order today?", deal responses like "Yes, accept this masala deal," and customer tagging like "Sharma aunty buys bread every Monday."

Text (Typing Input) is used for searching deals, manual stock entry via dropdowns, custom notes, and chatting with the AI in natural language.

Language Support: Primary languages for the hackathon demo are Hindi and English. The architecture supports all major Indian languages including Tamil, Telugu, Kannada, Marathi, Bengali, Gujarati, Malayalam, and Punjabi. AWS Bedrock handles translation and understanding natively.

---

### 5.11 Notification and Alert Delivery System

#### Hybrid Approach

The system uses both in-app experiences and WhatsApp-style alerts.

In the app, Deal Cards follow a Tinder-style swipe interface. When the shopkeeper opens the app, they see a stack of AI-personalized cards. Each card is a deal, alert, or recommendation. Swipe right to Accept or Acknowledge, swipe left to Dismiss, and tap to see full details.

Card types are color-coded: Red for Urgent Alerts like expiry warnings, Yellow for Trend Alerts like cricket match predictions, Green for New Deals from brands, Blue for Proactive Insights like unbranded conversion opportunities, and White for Reports like weekly savings summaries.

Push Notifications are used for time-sensitive items: urgent expiry alerts, trend alerts for events happening tomorrow, deal expirations, and delivery status updates.

WhatsApp Alerts are used for critical items when the user hasn't opened the app in 24 hours and for liquidation offer message generation that the shopkeeper forwards to customer groups.

Alert Frequency Management: Maximum 3 push notifications per day. AI ranks alerts by financial impact. Low-priority insights are queued in the Deal Cards stack, not pushed. Users can set Quiet Hours with default being no notifications before 7 AM or after 10 PM.

---

### 5.12 Onboarding Flow

The first-time setup takes a maximum of 5 minutes. Step 1 is downloading the app and signing up with a phone number via OTP. Step 2 is basic store info including optional store name, location via GPS auto-detect or manual pin, store type, and preferred language. Step 3 is the AI welcome and initial scan where the AI asks the user to take a photo of their main shelf, processes it, and confirms the categories detected. Step 4 is optional detailed setup where the user can voice-list their main products. Step 5 is the AI beginning to learn, instructing the user to take a photo of each shelf once a day.

Progressive Learning: In Week 1, the AI establishes a baseline inventory from photos and voice inputs. In Week 2, it begins calculating velocity. In Week 3, it starts making proactive suggestions. From Month 2 onward, it has full understanding of the store's patterns and makes highly accurate predictions.

---

### 5.13 Offline Mode

Features available offline include taking shelf photos that are saved locally and queued for upload, recording voice notes saved locally and queued for processing, viewing the previous 7 days of AI reports that are cached locally, viewing current active deals and their details, and manual stock logging that syncs when online.

Features requiring online connectivity include AI analysis of new photos (requires AWS Bedrock), Trend Hunter alerts (requires external data), deal discovery and acceptance (requires server communication), ONDC order placement, and WhatsApp message generation.

When connectivity is restored, all queued items auto-sync in the background. The user sees a syncing indicator. The AI processes the backlog and delivers consolidated results.

---

## 6. Non-Functional Requirements

### 6.1 Performance

Shelf photo analysis response time should be under 15 seconds. Voice command processing time should be under 5 seconds. Deal card loading time should be under 3 seconds. App launch to usable state should be under 4 seconds. ONDC order placement should be under 10 seconds.

### 6.2 Scalability

The system should support 10,000 concurrent retailer users initially, scalable to 1 million. It should support 500 concurrent brand dashboard users initially, scalable to 10,000. It should process 50,000 photos per day initially, scalable to 5 million. It should process 30,000 voice notes per day initially, scalable to 3 million.

### 6.3 Availability

System uptime target is 99.5%. Planned maintenance window is 2 AM to 4 AM IST. Data backup frequency is every 6 hours. Disaster recovery uses multi-AZ deployment on AWS.

### 6.4 Security

Authentication uses phone OTP-based login with no passwords. Data encryption in transit uses TLS 1.3 for all API calls. Data encryption at rest uses AES-256. Retailer data is isolated so the brand dashboard NEVER sees raw retailer PII without explicit consent. Shelf photos are stored in encrypted S3 buckets and auto-deleted after 90 days unless flagged for Trade-Promo verification. The system is aligned with India's Digital Personal Data Protection Act (DPDPA) 2023.

### 6.5 Usability

Maximum onboarding time is 5 minutes from download to first shelf scan. The UI is designed for semi-literate users and must work with zero reading if using voice and camera only. Large tap targets of minimum 48x48dp, high contrast text, and voice-first design ensure accessibility. Language can be switched with one tap at any time. Error messages are always in the user's chosen language and always conversational, not technical.

### 6.6 Compatibility

The app requires Android 8.0 or higher, which covers over 95% of Indian Android phones. Minimum RAM is 2 GB. Camera requirement is a rear camera of 8MP or higher. The app works on 3G, 4G, and 5G with offline mode for core features. The brand dashboard works on Chrome, Firefox, and Edge latest 2 versions and is responsive for tablet.

---

## 7. Business Model and Revenue

### 7.1 Revenue Model: Freemium

The Retailer App is 100% FREE forever. The Kirana owner never pays a single rupee.

The Brand Dashboard has three tiers. The Free tier at ₹0 per month allows posting up to 2 bounties per month, seeing limited aggregated market data, and viewing up to 10 matched retailers. The Growth tier at ₹4,999 per month includes unlimited bounties, the full market intelligence dashboard, Retailer Power Scores, sampling campaigns for up to 500 stores, and geographic demand heatmaps. The Enterprise tier at ₹19,999 per month includes everything in Growth plus custom cohort analysis, API access for integration with the brand's own systems, a dedicated account manager, priority matching, competitor intelligence reports, and white-label sampling campaigns.

### 7.2 Additional Revenue Streams

Transaction Commission: 2-3% commission on every deal value transacted through the platform, paid by the brand. This is the primary revenue driver.

Promoted Bounties: Brands can pay ₹500 to ₹5,000 extra to have their deals shown as Featured at the top of retailer Deal Cards, similar to sponsored ads.

Data Licensing: Aggregated, anonymized demand intelligence sold to large FMCG companies, market research firms, and government agencies as high-value B2B contracts.

ONDC Logistics Margin: A small margin on logistics facilitation provides supplementary revenue.

### 7.3 TAM, SAM, SOM

Total Addressable Market: 12 million+ Kirana stores multiplied by approximately ₹500 average platform revenue per month equals approximately ₹72,000 Crore per year (about \$8.6 billion).

Serviceable Addressable Market: 2 million stores in Tier 1 and 2 cities with smartphones equals approximately ₹12,000 Crore per year.

Serviceable Obtainable Market for Year 1: 10,000 stores multiplied by ₹1,000 per month equals ₹12 Crore per year.

---

## 8. Competitive Analysis

### 8.1 Direct Competitors

Udaan is a B2B wholesale marketplace where retailers order from big brands and distributors. It is a catalog-based ordering app with no AI intelligence, no demand forecasting, no visual shelf analysis, and no brand-retailer direct negotiation. It still relies on traditional distribution. Vyapar-IQ is an AI-first intelligence platform that eliminates middlemen.

Jumbotail operates a B2B supply chain for groceries. Similar to Udaan, it focuses on supply chain efficiency rather than retailer intelligence. It has no AI agents and no Trade-Promo feature.

Khatabook and OkCredit are digital ledgers for tracking customer credit. They solve a completely different problem — tracking debts, not inventory or demand. They have no AI and are passive record-keepers. They could be complementary to Vyapar-IQ.

Tally and Busy are accounting and billing software. They are too complex for Kirana owners, desktop-first, have no AI, no visual input, and require significant data entry.

### 8.2 Indirect Competitors

RetailPulse provides AI-powered retail analytics using shelf images for FMCG brands. It sells to brands (B2B), not to retailers. It analyzes shelf presence from the brand's perspective. Vyapar-IQ serves the retailer first and creates a marketplace.

Bizom is a retail intelligence platform for FMCG sales teams. It's used by brand field sales representatives, not by Kirana owners directly. Different user, different problem.

Amazon and Flipkart Wholesale focus on price-based ordering. They have no AI intelligence for the retailer, no local demand forecasting, and no margin optimization. They are viewed as a threat by Kirana owners, not an ally.

### 8.3 Competitive Moat

Data Network Effect: More retailers means more demand data, which means better AI predictions, which means more brands join, which means better deals for retailers, which means more retailers join. This creates a flywheel effect.

AI-Generated Intelligence: No competitor provides real-time hyperlocal demand forecasting combined with visual shelf analysis combined with brand matchmaking in a single platform.

Voice-First Zero-Literacy Design: Designed from the ground up for semi-literate users. Competitors require significant typing and reading.

Small Brand Focus: Competitors focus on big brands like Unilever, Nestle, and ITC. Vyapar-IQ deliberately focuses on small and emerging brands that are underserved and offer higher margins.

Retailer-First Philosophy: Every competitor puts the brand first because brands pay. Vyapar-IQ puts the retailer first with a free app, which builds trust and drives adoption. Brands follow the retailers.

---

## 9. Why AI is Needed — Not Rule-Based Logic

This section directly addresses the hackathon requirement that teams should clearly explain WHY AI is needed, not just HOW it is used.

### 9.1 Feature-by-Feature Justification

For Shelf Analysis, you cannot write IF/THEN rules to count products in a photograph where products overlap, have varied orientations, different lighting, and infinite SKU variations. Multimodal AI can perceive, identify, and count objects in images the way a human would but instantly and consistently.

For Demand Forecasting, consumer demand depends on 10+ simultaneous factors. Writing rules for every possible combination of weather, events, festivals, demographics, day of week, and payday cycles is computationally infeasible with millions of IF/THEN branches. AI correlates multiple unstructured signals simultaneously without anyone explicitly coding each combination.

For Expiry Date OCR, rules cannot read text from an image. Multimodal AI performs OCR natively.

For Brand-Retailer Matching, matching requires understanding product categories, consumer behavior, geographic relevance, margin economics, and retailer capabilities simultaneously. AI uses semantic understanding to reason that a store selling lots of loose rice is relevant for packaged rice brands without hardcoded rules.

For Dynamic Pricing, the optimal discount depends on product type, remaining shelf life, stock level, historical velocity, customer price sensitivity, and competitive pricing. AI calculates the optimal discount dynamically for each situation.

For Multi-Language Voice Understanding, rule-based NLP for Hindi, Tamil, Telugu and other languages is brittle and fails on dialects, accents, slang, and code-switching. Large Language Models handle multilingual input natively with high accuracy.

For Cohort-Based Sampling Analysis, determining whether poor results are due to product quality versus retailer non-compliance requires statistical reasoning across hundreds of data points that is not possible with simple rules.

For Proactive Brand Discovery, finding that a store selling 200kg loose rice should be offered packaged rice requires semantic reasoning about product categories and consumer substitution patterns.

### 9.2 The AI Chain — How Agents Work Together

The real power is the chain reaction between agents. The Shelf Analyst detects dead stock in Brand X chips. The Trade-Promo Agent reasons that the retailer should stop buying Brand X and that Brand Y is offering 30% margin on their chips. The Trend Hunter adds that a cricket match is tomorrow and chips demand will spike, making this the perfect time to trial Brand Y. The Liquidation Bot adds that remaining Brand X chips should be cleared with a 20% discount today, freeing shelf space for Brand Y tomorrow. The result is that the shopkeeper clears dead stock, tries a high-margin alternative, tests it on a high-demand day, and maximizes profit. This multi-step reasoning across multiple data sources is fundamentally an AI problem.

---

## 10. AWS Services Mapping

Amazon Bedrock with Claude 3.5 Sonnet Multimodal is the core AI engine. It processes shelf images for object detection, counting, OCR, and placement analysis. It processes voice transcriptions. It performs reasoning for all 4 agents. It generates natural language responses in multiple Indian languages.

Amazon Bedrock Titan Embeddings converts product descriptions, brand profiles, and retailer profiles into vector embeddings for semantic matching.

Amazon Bedrock Knowledge Bases stores the product catalog, brand bounty database, and retailer profiles, enabling RAG (Retrieval Augmented Generation).

Amazon S3 stores shelf photos, voice note audio files, and AI-generated reports with encryption at rest.

Amazon Transcribe converts voice notes in Hindi, English, and regional languages to text.

Amazon Polly converts AI text responses back to speech for voice-output alerts.

Amazon DynamoDB provides the NoSQL database for real-time inventory state, retailer profiles, deal status, and Power Scores.

Amazon RDS with PostgreSQL provides the relational database for brand dashboard data, bounty management, sampling campaigns, and analytics.

AWS Lambda provides serverless compute for event-driven processing.

Amazon API Gateway provides REST and WebSocket APIs for mobile app and brand dashboard communication.

Amazon SNS provides push notifications to the retailer mobile app.

Amazon EventBridge provides scheduled events for Trend Hunter scans every 6 hours and expiry checks every morning.

Amazon CloudWatch provides monitoring, logging, and alerting.

AWS Amplify provides frontend hosting for the Brand Web Dashboard.

Amazon Cognito provides user authentication via phone OTP for retailers and email/password for brands.

Amazon Location Service provides geolocation for store pinning, geographic demand heatmaps, and ONDC logistics routing.

---

## 11. Success Metrics and KPIs

### Retailer Metrics (6-month targets)

Monthly Active Retailers: 10,000. Average spoilage reduction per store: 60%. Average margin improvement per store: +₹5,000 per month. Trend alert accuracy: above 75%. Retailer 30-day retention: above 70%. Shelf scans per retailer per week: above 5. Deal acceptance rate: above 30%.

### Brand Metrics (6-month targets)

Active brands on platform: 500. Average retailer reach per bounty: 200 stores. Deal-to-reorder conversion rate: above 40%. Sampling campaign ROI: above 3x. Brand quarterly retention: above 60%.

### Platform Metrics (6-month targets)

Gross Merchandise Value: ₹5 Crore per month. Platform revenue: ₹15 Lakh per month. ONDC orders fulfilled: 5,000 per month. AI shelf analysis accuracy: above 85%. System uptime: above 99.5%.

---

## 12. Risks and Mitigations

Low retailer adoption where Kirana owners are resistant to new technology is mitigated by voice-first zero-literacy design, the app being free forever, showing immediate value through the first spoilage save, and grassroots onboarding through distributor relationships.

Low brand adoption where small brands don't know about the platform is mitigated by starting with local brands in 1-2 cities, showing clear ROI data, and offering a free tier to try.

AI shelf analysis inaccuracy from miscounting items in cluttered shelves is mitigated by using high-resolution images, allowing user correction, and having the AI learn from corrections.

Internet connectivity issues in rural areas with poor 3G/4G is mitigated by a robust offline mode with queue and sync functionality and minimal data transfer through image compression.

ONDC logistics unreliability from delivery delays or failures is mitigated by allowing brands to use their own logistics as fallback and showing estimated delivery time before order confirmation.

Retailer gaming the system through faking shelf photos or not distributing samples is mitigated by cohort-based analysis catching outliers, natural economic incentives preventing most fraud since money stuck in unsold stock hurts the retailer, and the Power Score self-regulating behavior.

Brand product quality issues hurting retailer trust is mitigated by cohort-based sampling analysis identifying bad products quickly and the platform flagging or removing brands with consistently poor conversion.

Data privacy breach where retailer data is exposed to brands is mitigated by strict anonymization, retailer identity revealed only on explicit opt-in, encrypted storage, and DPDPA compliance.

Competitor response from Udaan or Jumbotail adding similar features is mitigated by the data network effect moat, first-mover advantage in the AI-for-Kirana space, and focus on small brands that competitors ignore.

---

## 13. Ethical AI and Responsible Design

### Data Privacy

Informed Consent means the retailer is told exactly what data is collected and how it's used during onboarding in plain language. Data Minimization means we collect only what's needed and raw photos are deleted after 90 days. Anonymization means the brand dashboard never shows retailer PII without explicit consent. Right to Deletion means the retailer can delete their account and all data at any time. No Third-Party Sharing means raw data is never sold and only aggregated anonymized intelligence is available.

### AI Fairness

No Bias in Deal Matching means the AI matches based on product relevance, geography, and store profile, not based on religion, caste, gender, or personal characteristics. Equal Opportunity means new stores with low Power Scores still receive deal opportunities with the score affecting priority not exclusion. Transparent Scoring means retailers can see their Power Score and understand what factors affect it. Brand Product Neutrality means the AI does not favor one brand over another unless the brand has paid for a Promoted Bounty which is clearly marked as Sponsored.

### Responsible AI Behavior

Honesty means the AI never claims certainty when unsure and uses phrases like "I think" or "Based on my analysis" or "I'm not sure about this." No Hallucination means if the AI cannot read a label or count items clearly it asks the user instead of guessing. Human-in-the-Loop means all major actions require explicit user confirmation. No Manipulation means the AI does not use dark patterns to pressure retailers. Explainability means every recommendation comes with a reason.

### Environmental Responsibility

The Liquidation Bot directly reduces food spoilage aligning with UN SDG 12 on Responsible Consumption and Production. Serverless architecture means only consuming compute resources when processing requests. ONDC integration optimizes delivery routes reducing fuel consumption.

---

## 14. Glossary

Kirana: A small family-owned retail shop in India, typically 200-500 sq ft, selling groceries, FMCG products, and daily essentials.

Dead Capital or Dead Stock: Inventory that sits unsold for extended periods, tying up the shopkeeper's cash.

FMCG: Fast-Moving Consumer Goods — products that sell quickly at relatively low cost.

Trade Promotion: Marketing practice where brands incentivize retailers through higher margins, bonuses, or free goods to prominently display and actively sell their products.

Brand Visibility: The degree to which a brand's product is visible to customers in a retail store.

ONDC: Open Network for Digital Commerce, India's government-backed open protocol for e-commerce and logistics.

Power Score: Vyapar-IQ's AI-generated reputation score for retailers.

Bounty: A deal posted by a brand on Vyapar-IQ.

Cohort Analysis: Statistical method of analyzing a group of stores together to determine whether an outcome is due to individual behavior or a systemic factor.

Velocity: The rate at which a product sells.

Default Dispensing: When a customer doesn't specify a brand, the retailer gives the product from the collaborated brand earning higher margin.

SKU: Stock Keeping Unit, a unique identifier for each distinct product.

MRP: Maximum Retail Price, the highest legal selling price in India.

OCR: Optical Character Recognition, AI technique to read printed text from images.

RAG: Retrieval Augmented Generation, AI technique where the model retrieves relevant data before generating a response.

UPI: Unified Payments Interface, India's real-time payment system.

DPDPA: Digital Personal Data Protection Act 2023, India's primary data privacy legislation.
