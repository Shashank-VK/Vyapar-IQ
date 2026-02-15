# Vyapar-IQ — Design Document
## "The AI Profit-Guard for Kiranas"
### Hack to Skill Hackathon | Powered by AWS | AI for Bharat

---

## Table of Contents

1. System Architecture Overview
2. High-Level Architecture Diagram
3. Component Deep Dive
4. Data Architecture
5. AI/ML Architecture
6. API Design
7. User Interface Design
8. Offline Architecture
9. Security Architecture
10. Deployment Architecture
11. Demo Script for Hackathon

---

## 1. System Architecture Overview

### 1.1 Architecture Principles

**AI-First:** Every feature is powered by AWS Bedrock. No rule-based logic for core decisions. The AI reasons, infers, and recommends based on multiple data signals simultaneously. A single multimodal model (Claude 3.5 Sonnet) powers all four agents — vision, language, reasoning, and generation — eliminating the need for multiple fragmented ML pipelines.

**Serverless:** AWS Lambda for all compute. The platform pays only for what it uses and auto-scales from 10 stores to 10 million stores without any infrastructure changes. No idle servers, no capacity planning.

**Event-Driven:** Actions trigger chains of events. A photo upload triggers shelf analysis, which triggers an expiry alert, which triggers a liquidation offer, which triggers a WhatsApp message generation. A deal acceptance triggers an ONDC order, which triggers a brand notification. This chain reaction is orchestrated by the AI Reasoning Engine.

**API-First:** All communication happens through RESTful APIs via Amazon API Gateway. The mobile app and web dashboard are independent clients consuming the same backend services. This allows building either interface independently and enables future third-party integrations.

**Offline-Resilient:** The mobile app works offline for core features like taking photos, recording voice notes, and viewing cached reports. Everything queues locally with timestamps and auto-syncs when connectivity returns. This is critical for rural India where internet connectivity is patchy.

**Privacy-by-Design:** Data anonymization is built into the architecture from the start, not bolted on later. Every API endpoint that serves data to the brand dashboard passes through an anonymization layer. Retailer identity is never exposed to brands without explicit retailer consent.

### 1.2 Technology Stack

**Retailer Mobile App:** React Native for Android-first development. This provides cross-platform capability with a large developer ecosystem and is easy to scaffold with AI-assisted vibe coding tools. Local storage uses AsyncStorage and SQLite for offline queuing. Camera access uses react-native-camera. Audio recording uses react-native-audio-recorder-player. Push notifications use Firebase Cloud Messaging triggered by Amazon SNS. Navigation uses React Navigation with bottom tab and stack navigators.

**Brand Web Dashboard:** React.js for the single-page application hosted on AWS Amplify, which provides built-in hosting, authentication integration, and API connectivity. UI components use Ant Design or Material UI. Charts use Recharts. Maps use Leaflet.js with Amazon Location Service tiles.

**API Layer:** Amazon API Gateway handles all REST API routing with rate limiting, throttling, and authorization. AWS Lambda functions in Python handle all business logic and AI orchestration. Each agent has its own Lambda handler function for clean separation of concerns.

**Core AI Engine:** Amazon Bedrock is the centerpiece. Claude 3.5 Sonnet (Multimodal) handles all perception (reading shelf photos), reasoning (demand forecasting, pricing optimization, brand matching), and generation (natural language responses in multiple languages, WhatsApp messages). Titan Embeddings handle vector representations for semantic search — matching brands with retailers, categorizing products, and powering the knowledge base retrieval. Bedrock Knowledge Bases store the product catalog, brand bounty database, retailer profiles, and store memory, enabling RAG (Retrieval Augmented Generation) so the AI always has relevant context when making decisions.

**Voice Processing:** Amazon Transcribe converts Hindi, English, and regional language speech to text with support for Indian accents and code-switching (mixing Hindi and English in the same sentence). Amazon Polly converts AI text responses back to natural-sounding speech for voice-output alerts.

**Primary Real-Time Database:** Amazon DynamoDB provides single-digit millisecond reads and writes for inventory state, store profiles, deal status, alerts, voice note processing results, and Power Scores. On-demand capacity mode means it auto-scales with usage.

**Analytics Database:** Amazon RDS PostgreSQL provides relational storage for brand dashboard data — bounty configurations, sampling campaigns, market intelligence aggregates, conversion funnel analytics, and platform financial transactions.

**Object Storage:** Amazon S3 stores all binary objects — shelf photos, voice note audio files, AI-generated marketing materials, and analysis result documents. All objects are encrypted with AES-256 at rest.

**Vector Store:** Amazon OpenSearch Serverless stores vector embeddings generated by Titan Embeddings, powering the RAG pipeline for Knowledge Bases and semantic search for brand-retailer matching.

**Push Notifications:** Amazon SNS triggers Firebase Cloud Messaging for Android push notifications.

**Scheduled Jobs:** Amazon EventBridge runs cron-style scheduled triggers — Trend Hunter scans every 6 hours, expiry checks every morning at 6 AM, Power Score recalculation every Sunday midnight.

**WhatsApp Integration:** Twilio WhatsApp Sandbox provides quick setup for the hackathon demo. It sends critical alerts and liquidation offer messages via WhatsApp. For production, this would upgrade to the full WhatsApp Business API.

**Authentication:** Amazon Cognito handles phone OTP-based login for retailers (zero passwords) and email/password login for brand dashboard users.

**Geolocation:** Amazon Location Service provides store location pinning, geographic demand heatmap generation, and ONDC logistics routing support.

**Monitoring:** Amazon CloudWatch provides centralized logging, metrics, and alerting for all services.

---

## 2. High-Level Architecture Diagram

The architecture is organized in five horizontal layers, flowing from top (user-facing) to bottom (external integrations).

### Layer 1 — User Interfaces

On the left side is the Retailer Mobile App for Android built with React Native. It features three input modes represented by a camera icon, microphone icon, and text/keyboard icon. It includes a Deal Cards interface for the Tinder-style swipe experience and a notification system. It has an offline queue for storing data when connectivity is unavailable.

On the right side is the Brand Web Dashboard built with React.js and hosted on AWS Amplify. It features Market Intelligence with heatmaps and trends, Bounty Management for creating and tracking deals, Sampling Campaign management, Retailer Discovery with anonymized profiles, and Analytics and Reports with exportable data.

Both interfaces connect downward to Layer 2.

### Layer 2 — Gateway and Authentication

Amazon API Gateway sits in the center, receiving all REST API requests from both interfaces. It handles routing, rate limiting, and throttling. Connected to it is Amazon Cognito which authenticates every request — phone OTP tokens for retailer requests, email/password JWT tokens for brand requests. All requests must pass authentication before reaching Layer 3.

### Layer 3 — Intelligence Layer (The Brain)

This is the largest and most critical layer. It contains several components working together.

**AWS Lambda Functions** are organized as individual handlers for each agent and supporting service:

The Shelf Analyst Handler is triggered when a new shelf photo is uploaded to S3. It retrieves store context from DynamoDB, calls Bedrock for multimodal analysis of the image, processes the AI response into structured inventory data, updates the inventory state in DynamoDB, and triggers downstream agents (Liquidation Bot for expiry items, Brand Discovery for unbranded goods detected, Trade-Promo for placement verification).

The Trend Hunter Handler is triggered by EventBridge every 6 hours. It fetches weather data from the Weather API, news and events from RSS feeds, and festival calendar data. For each active store, it retrieves the store context and historical velocity data, then calls Bedrock to reason about demand implications. If the AI predicts a significant demand change (more than 30% with confidence above 65%), it generates an alert and pushes it through the Notification Handler.

The Liquidation Bot Handler is triggered both by EventBridge every morning at 6 AM and by events from the Shelf Analyst when expiring items are detected. It queries DynamoDB for items expiring within 3 days, calls Bedrock to calculate optimal pricing strategies, generates localized WhatsApp message content, and delivers alerts through the Notification Handler.

The Trade-Promo Agent Handler has multiple triggers. When a brand creates a new bounty, it matches the bounty with relevant retailers by querying store data and calling Bedrock for relevance scoring using Titan Embeddings. When a retailer opens the Deals tab, it fetches personalized deal cards. When the Shelf Analyst detects unbranded volume or dead stock, it triggers the Brand Discovery sub-component. When a reorder is detected (a store places a repeat order for a collaborated product), it updates the Power Score components and notifies the brand dashboard.

**The AI Reasoning Engine (Orchestrator)** sits at the center of all agent activity. It maintains a Store Context Manager that holds the complete memory for each store — current inventory state, historical velocity data, active deals and collaborations, demographic profile, shopkeeper preferences and patterns, and past AI interactions and corrections. The Agent Orchestrator coordinates when multiple agents produce outputs simultaneously. It gathers context, runs relevant agents in parallel, collects their outputs, resolves conflicts (for example if the Shelf Analyst says "reorder chips" but the Trend Hunter says "chip demand is dropping next week"), prioritizes recommendations by financial impact, generates a unified response, and routes it to the appropriate delivery channel. The Conflict Resolution Logic follows four rules: (1) Financial impact ranking where higher rupee amounts get priority; (2) Time sensitivity where more urgent items rank higher; (3) Confidence level where the AI's own certainty affects ranking; (4) No contradictions where if two agents give opposing advice, the AI explains both sides and gives nuanced guidance instead of conflicting alerts.

**Supporting sub-components** within the Trade-Promo Agent include:

The Brand Matching Engine uses Titan Embeddings to create vector representations of brand bounties and retailer profiles. It performs semantic similarity search to find the best matches, considering geography, product category, demographic fit, category strength, and Power Score. It generates relevance scores for each potential match.

The Sampling Engine manages the lifecycle of sampling campaigns — from store selection using AI-powered ranking, through sample distribution via ONDC, to 14-day cohort tracking and conversion analysis. It implements the critical cohort-based analysis that fairly attributes non-conversion to either product quality issues or individual store behavior.

The Power Score Engine recalculates retailer reputation scores every Sunday, combining five weighted components: Sales Velocity (30%), Shelf Compliance (20%), Reorder Consistency (20%), Category Strength (15%), and Sampling Trust (15%). It assigns badges (Platinum, Gold, Silver, Bronze, Growing) and determines deal tier eligibility.

The Brand Discovery Engine runs proactively when the Shelf Analyst detects opportunities. It identifies stores selling high volumes of loose or unbranded products and searches the platform's brand database for matching packaged alternatives. When a match is found, it generates proactive deal cards for the retailer and opportunity alerts for the brand. When no match is found, it generates a market opportunity alert on the brand dashboard to attract new brands.

**The Notification Handler** receives notification requests from all agents and services. It implements a priority queue that ranks notifications by financial impact and time sensitivity. It enforces daily limits (maximum 3 push notifications per day, maximum 1 WhatsApp alert per day, unlimited in-app deal cards). It routes to the appropriate channel: Amazon SNS for push notifications, Twilio for WhatsApp, and DynamoDB for in-app deal card queuing. It enforces quiet hours (default no notifications before 7 AM or after 10 PM) with an exception for extremely urgent items like stock expiring today.

**The ONDC Integration Handler** is triggered when a retailer accepts a deal. It constructs an ONDC-compatible order object with pickup location (brand warehouse), drop location (retailer store via Amazon Location Service geocoding), items and quantities, and payment method. It sends the order to the ONDC network for logistics provider matching, receives status webhooks (confirmed, picked up, in transit, delivered), updates DynamoDB with delivery status, sends push notifications at each status change, and on delivery confirmation, updates the retailer's inventory and activates Trade-Promo monitoring.

### Layer 4 — Data Layer

**Amazon DynamoDB** stores all real-time operational data organized in several tables:

The Stores table holds complete retailer profiles including owner information, store location with GPS coordinates, store type, preferred language, demographic profile (AI-inferred), category strength scores (per-category 1-10 ratings), overall Power Score with badge, Sampling Trust Score, list of active collaboration deal IDs, notification preferences, and onboarding status.

The Inventory table holds per-store per-item records including product name and brand, category and subcategory, whether it is branded or loose, current quantity and unit, cost price and selling price and MRP, margin percentage, expiry date with the source of that data (OCR, voice input, text input, or AI prompt with confirmation), daily average velocity and trend direction, dead stock flag with the date it was flagged, Trade-Promo deal ID if applicable, last update timestamp and source, reorder frequency in days, and 30-day totals for units sold and revenue.

The ShelfScans table stores records for every shelf photo analyzed including the S3 key for the original photo, the previous scan ID for comparison, counts of items detected grouped by velocity category (high, dead, expiring), Trade-Promo verification results, average AI confidence score, any user corrections applied, the S3 key for the full analysis result document, and processing time.

The Deals table tracks every brand-retailer deal including the bounty ID it originated from, brand and store identifiers, deal type (standard, trade promotion, sampling, volume, conversion), product details and pricing, margin information, Trade-Promo details (required placement, verified placement, placement score, verification photo), deal status, the complete reorder history with dates and quantities, total units ordered and GMV generated, ONDC order IDs for each delivery, and payment method and status.

The Alerts table stores every notification generated including alert type and source agent, priority level and financial impact amount, card color for UI rendering, title and multilingual summaries, full details reference, recommendations list, creation and expiration timestamps, read status, action taken by the retailer, and post-event validation data (to measure prediction accuracy).

The VoiceNotes table stores voice interaction records including the audio S3 key, duration, detected language, transcription text, AI-classified intent (stock update, query, command, deal response), extracted structured data (product, quantity, expiry date), AI response text, processing status, and whether inventory was updated as a result.

**Amazon RDS PostgreSQL** stores all analytical and brand-facing data:

The brands table holds brand profiles with contact information, product categories, warehouse locations, subscription tier and expiration, and aggregate performance metrics.

The bounties table holds complete bounty configurations including product details, margin offered, targeting criteria (geography, demographics, category strength, minimum Power Score), budget and duration, performance metrics (matched, accepted, reordered counts, total GMV, average placement score), and promoted bounty flag.

The sampling_campaigns table holds campaign definitions with product details, samples per store count, target store count, targeting criteria, distribution instructions, tracking duration and dates, conversion results with cohort verdict, geographic and demographic insights, total cost and resulting GMV and ROI multiplier.

The sampling_store_assignments table tracks individual store participation in campaigns with delivery status, ONDC order reference, conversion tracking (whether the store ordered the paid product, first order date, total units ordered), and sampling trust impact score.

The market_intelligence table stores pre-aggregated anonymized data organized by geography, category, and time period. Each record includes total weekly volume, the split between branded and unbranded sales, volume trend over 4 weeks, primary demographic for the area, a conversion opportunity score (1-10 rating of how ripe the area is for branded conversion), and competition level.

The platform_transactions table records all platform revenue events including deal commissions, promoted bounty fees, subscription payments, and sampling costs.

**Amazon S3** is organized into bucket prefixes: shelf-photos for raw shelf images organized by store ID and date, voice-notes for audio files organized by store ID and date, scan-results for full AI analysis JSON documents, verification for Trade-Promo shelf placement photos, alerts for full alert detail documents, and exports for generated brand dashboard reports.

**Amazon OpenSearch Serverless** stores vector embeddings for three primary use cases: product embeddings (converting product descriptions into vectors for category matching and similarity search), brand bounty embeddings (converting bounty descriptions into vectors for retailer matching), and retailer profile embeddings (converting store characteristics into vectors for brand discovery).

### Layer 5 — External Integrations

**ONDC Network** provides the logistics backbone. Vyapar-IQ sends standardized order objects via ONDC protocol. The ONDC network matches with available logistics providers based on route, capacity, and cost. Delivery status is reported back via webhooks. For the hackathon demo, this can be simulated with mock API responses while the architecture correctly shows the integration point.

**Twilio WhatsApp Sandbox** provides WhatsApp messaging capability. It is used for two purposes: sending critical alerts to retailers who haven't opened the app (urgent expiry warnings, time-sensitive trend alerts), and sending pre-formatted liquidation discount messages that retailers forward to their customer WhatsApp groups.

**OpenWeatherMap API** provides weather forecast data. The Trend Hunter queries this every 6 hours for each city where active stores are located, fetching 48-hour forecasts including temperature, precipitation probability, humidity, and weather conditions.

**News and Events APIs** provide external context data. RSS feeds from major news outlets are scanned for local events. Sports calendars (cricket schedules from ESPN Cricinfo or similar sources) are checked for upcoming matches. A pre-loaded Indian festival database organized by region, religion, and date provides cultural context. All of this feeds into the Trend Hunter's reasoning process.

### Data Flow Paths

**Path 1 — Shelf Scan Flow:**
Retailer takes photo → Photo uploads to S3 → S3 event triggers Shelf Analyst Lambda → Lambda fetches store context from DynamoDB and previous scan photo from S3 → Lambda sends current photo + previous photo + context to Bedrock Claude → Bedrock analyzes and returns structured results → Lambda updates Inventory table in DynamoDB → Lambda stores full analysis in S3 → If expiry items detected, Lambda triggers Liquidation Bot → If unbranded items detected, Lambda triggers Brand Discovery → If Trade-Promo product detected, Lambda runs verification → Results pushed to retailer via API Gateway → Push notification sent if urgent items found.

**Path 2 — Trend Alert Flow:**
EventBridge triggers Trend Hunter Lambda every 6 hours → Lambda fetches weather from OpenWeatherMap, news from RSS feeds, festival calendar data → For each active store, Lambda fetches context and history from DynamoDB → Lambda sends all data to Bedrock Claude for reasoning → If significant demand change predicted, Lambda generates alert → Alert stored in DynamoDB Alerts table → Notification Handler sends push notification via SNS and/or WhatsApp via Twilio → Alert appears as yellow card in retailer's Deal Cards.

**Path 3 — Brand Deal Flow:**
Brand creates bounty on web dashboard → API Gateway routes to Trade-Promo Lambda → Lambda stores bounty in RDS → Lambda calls Bedrock with Titan Embeddings to find matching retailers → Matching retailers identified from DynamoDB → Personalized deal cards generated and stored in DynamoDB → When retailer opens Deals tab, API returns personalized cards → Retailer swipes right → API triggers ONDC Integration Lambda → ONDC order created → Delivery tracked via webhooks → On delivery, inventory updated → Reorder monitoring begins → Brand dashboard updated with acceptance data.

**Path 4 — Proactive Discovery Flow:**
Shelf Analyst detects 15kg loose rice in a store → Triggers Brand Discovery Lambda → Lambda queries RDS for packaged rice brands with active bounties targeting this geography → If match found, Lambda calls Bedrock to generate a personalized pitch → Blue proactive insight card generated and stored in DynamoDB → Retailer sees the card: "You sell 200kg loose rice at ₹5/kg profit. Brand Y offers ₹12/kg." → Simultaneously, brand dashboard receives an opportunity notification.

**Path 5 — Liquidation Flow:**
Every morning at 6 AM, EventBridge triggers Liquidation Bot Lambda → Lambda queries DynamoDB Inventory for items expiring within 3 days → For each item, Lambda calls Bedrock to model discount scenarios → Bedrock calculates optimal pricing and generates WhatsApp message text → Alert stored in DynamoDB as red urgent card → Push notification sent → Retailer taps notification → Sees discount recommendation with financial breakdown → Taps "Send to WhatsApp" → App opens WhatsApp with pre-filled message → Retailer forwards to customer group → After expiry date, AI checks if stock was depleted → Outcome recorded for future optimization.

**Path 6 — Voice Interaction Flow:**
Retailer taps voice button and speaks in Hindi → Audio recorded and uploaded to S3 → Lambda triggered → Audio sent to Amazon Transcribe for speech-to-text → Transcribed text sent to Bedrock Claude with store context → Bedrock classifies intent (stock update, query, command, deal response) → If stock update: Inventory updated in DynamoDB → If query: AI generates answer from store context and knowledge base → If deal response: Deal status updated → Response text generated in Hindi → Optionally converted to speech via Amazon Polly → Response displayed in chat interface and/or spoken back.

**Path 7 — Power Score Calculation Flow:**
Every Sunday midnight, EventBridge triggers Power Score Lambda → For each active retailer, Lambda fetches velocity data from Inventory table, shelf compliance data from ShelfScans table, reorder data from Deals table, category data from Inventory table, and sampling data from RDS sampling_store_assignments table → Lambda calculates weighted score → Badge assigned based on threshold → DynamoDB Stores table updated → Retailers who moved up a tier receive a congratulatory notification → Brand dashboard retailer profiles auto-refresh with new scores.

---

## 3. Component Deep Dive

### 3.1 Retailer Mobile App — Detailed Screen Architecture

The app uses a bottom tab navigation bar with five tabs. The navigation flow supports both the standard tab switching and deep linking from push notifications (tapping a notification takes the user directly to the relevant screen).

**Authentication Flow:**
Phone number entry screen → OTP sent via Cognito → OTP verification screen → If first-time user, route to Onboarding Flow → If returning user, route to Home Tab.

**Onboarding Flow (First-time only, maximum 5 minutes):**
Step 1: Language selection with large flag-style buttons for Hindi, English, and regional languages. Step 2: Store information with store name (optional, can skip), location via GPS auto-detect with a manual pin-on-map fallback, and store type selection (General Store, Grocery, Dairy, Specific). Step 3: AI-guided first shelf scan where the AI voice says "Welcome! Let me learn about your store. Can you take a photo of your main shelf?" The user takes 1-3 photos of different sections. The AI processes them and confirms: "I can see you sell snacks, dairy, and staples. Is that correct? Do you also sell anything else?" Step 4: Optional voice listing where the user can say their main products. Step 5: AI commitment message: "I've set up your store. Take a photo of each shelf once a day, and I'll learn your patterns. I'll also start showing you deals from brands in your area."

**Home Tab — The Daily Command Center:**
The top area shows a personalized AI greeting in the user's language with a summary of today's key alerts (count of expiring items, trend alerts, new deals available). Below that is a prominent "This Week's Savings" banner showing the rupee amount the AI has helped save, calculated from spoilage prevention and margin improvements. Three large, tappable quick action buttons are centered: Scan Shelf (camera icon), Voice Note (microphone icon), and View Deals (handshake icon). Below the buttons is a scrollable Recent Activity feed showing the last 5-10 AI interactions as conversation-style bubbles.

**Scan Tab — The AI Eye:**
Opening this tab activates the camera with a viewfinder overlay showing alignment guides. After taking a photo, a processing screen shows an animated scanning effect while the AI works (typically 10-15 seconds). The results screen shows the photo at the top with colored bounding boxes around detected products (green for fast-selling, red for dead stock, orange for expiring). Below the annotated photo, results are grouped into sections: "Selling Fast — Reorder These" with green headers showing product names and daily velocity, "Dead Stock — Stop Buying" with red headers showing product names and days sitting idle, "Expiring Soon" with orange headers showing product names, expiry dates, days remaining, and a prominent "Create Offer" button that launches the Liquidation flow, "Trade-Promo Verified" with checkmark headers showing brand name, placement score out of 10, and margin bonus status, and "Unbranded Opportunities" with lightbulb headers showing detected loose items with a "Find Branded Deal" button. A "Scan Again" button at the bottom allows rescanning the same or a different shelf. Historical comparison is accessible by swiping left to see how the shelf has changed over the past week.

**Deals Tab — Tinder for Retail:**
The main area shows a large card in the center of the screen. Each card is a complete deal proposal. The card header shows the deal type with a color indicator (green for New Deal, blue for Proactive AI Discovery, purple for Sampling Opportunity). The card body shows the product image or brand logo, product name and size, a clear comparison between current margin and offered margin highlighted prominently, the customer impact (whether the price is same, lower, or higher for the end customer), delivery timeline via ONDC, and any special conditions (placement requirements for Trade-Promo, sampling instructions). The card footer shows two large action zones: the left half for swiping left or tapping to Dismiss, and the right half for swiping right or tapping to Accept. Below the active card, dot indicators show how many deals are available. A tab switcher allows toggling between Available Deals, Active Deals (currently running collaborations), and Deal History. When the retailer accepts by swiping right, a confirmation overlay appears with order details, delivery estimate, and payment method. Tapping Confirm triggers the ONDC order.

**Voice Tab — The AI Conversation:**
A large circular microphone button dominates the screen. The user holds to speak (push-to-talk) or can toggle to tap-to-start and tap-to-stop mode. While recording, a waveform animation provides visual feedback. After release, the AI processing indicator shows while Transcribe converts speech and Bedrock processes the intent. The conversation appears as chat bubbles: the user's transcribed speech on the right, the AI's response on the left. Below the microphone button is a text input field labeled "Or type here" for users who prefer typing. Suggested action chips appear above the input area: "What should I order today?", "Show my expiring items", "How is my Power Score?", "Accept the rice deal". These are tappable shortcuts. The full conversation history is scrollable, showing all past voice and text interactions.

**Profile Tab:**
Store Profile section shows the store name, location on a mini-map, store categories, and an edit button. Power Score section shows the overall score prominently as a large number with the badge (Platinum, Gold, Silver, Bronze, Growing). Below it, a breakdown shows each of the five components with progress bars and individual scores. A tooltip on each component explains what it measures and how to improve it. Active Collaborations section lists all current brand deals with brand name, product, margin earned so far, and days active. Earnings section shows a monthly bar chart of earnings from brand deals, spoilage savings, and trend-based extra revenue. Settings section includes language selection, notification preferences (quiet hours, maximum notifications per day, WhatsApp alerts toggle), and account management (logout, delete account).

### 3.2 Brand Web Dashboard — Detailed Screen Architecture

The dashboard uses a left sidebar navigation that is collapsible for more screen space. The top bar shows the brand name, notification bell with count, and profile dropdown.

**Overview Page:**
Four KPI cards across the top show Active Bounties count, Total Retailers Reached this month, Total GMV this month, and Overall ROI percentage. Below the KPIs, two primary widgets sit side by side. The left widget shows a mini demand heatmap for the brand's primary category. The right widget shows a conversion funnel from total bounties posted through matches, acceptances, and reorders. Below the widgets, an AI Insights section shows 2-3 automatically generated observations such as "Your masala performs 3x better in family areas than student areas. Consider redirecting budget." and "Reorder rate dropped 12% this week in North Bangalore. Weather was unusually hot — may recover next week." Action buttons link to Create New Bounty and View Full Intelligence Report.

**Bounty Management Page:**
The Create Bounty form is a multi-step wizard. Step 1 is Product Selection from the brand's catalog with the ability to add new products. Step 2 is Pricing and Margin Setup where the brand enters their wholesale price and the system calculates the retailer's margin based on MRP. Step 3 is Targeting where a map interface allows drawing geographic boundaries, demographic checkboxes allow selecting Family Area, Student Area, Premium Area, and Budget Area, a category strength slider sets the minimum score (1-10) required in the relevant category, and a minimum Power Score slider sets the retailer quality threshold. Step 4 is Deal Configuration where the deal type is selected (Standard, Trade Promotion with placement requirements, Volume Deal with tier pricing, Conversion deal targeting unbranded sellers). Step 5 is Budget and Duration where the total budget and date range are set. Step 6 is Review and Launch showing a preview of how the deal card will appear to retailers. The Active Bounties list shows each bounty as an expandable card with metrics: matched count, accepted count, reorder count, and GMV. Expanding a card reveals a geographic breakdown of performance, a timeline of acceptance over the bounty duration, a visual verification gallery showing shelf photos submitted by retailers with AI-generated placement scores displayed on each photo, and individual retailer performance (anonymized until the retailer has accepted the deal).

**Sampling Campaigns Page:**
The Create Campaign form includes product and sample specification (what the sample is, quantity per store), AI-recommended store selection where the AI pre-selects the optimal stores based on category relevance, Power Score, footfall estimate, and geographic fit — the brand can review and adjust, distribution instructions as a free-text field with templates ("Give 1 sample with every purchase above ₹100"), and tracking configuration with duration (default 14 days) and success criteria. Active Campaigns show a dashboard with a progress bar for distribution status, a real-time conversion counter updating as stores place paid orders, and a cohort health indicator showing whether the overall conversion rate suggests product success or quality concerns. Completed Campaigns show a comprehensive results page with the overall conversion rate and cohort verdict (Product Success, Needs Improvement, Inconclusive), a geographic heatmap showing where conversion was highest and lowest, a demographic breakdown showing which types of areas responded best, individual store performance (only for stores that opted in to share details), and an AI-generated summary with recommendations for the next campaign.

**Retailer Discovery Page:**
The main view is a full-width map with anonymized store pins. Pin colors indicate Power Score tier: gold pins for Platinum and Gold retailers, silver for Silver, and gray for Bronze and Growing. Clicking a pin opens a side panel with the anonymized retailer profile showing the overall Power Score with badge, a radar chart showing all five score components, category strength bars showing which product categories the store excels in, demographic label (Family Area, Student Area, etc.), monthly volume estimate (High, Medium, Low), number of active collaborations, and a "Create Targeted Bounty for This Retailer" button. Filter controls allow narrowing by geography (draw on map or select areas), category strength (slider per category), demographic profile (checkboxes), minimum Power Score (slider), and current collaboration status (available, already collaborating with competing brands). An AI-powered "Recommended for You" section appears at the top when the brand logs in, showing a curated list of 10-20 retailers that are the strongest fit based on the brand's product category and past campaign performance.

**Market Intelligence Page:**
The Demand Heatmap is an interactive map where the brand selects a product category from a dropdown. The map shades areas by demand intensity (red for high, yellow for medium, green for low). Hovering over an area shows the estimated weekly volume and the branded versus unbranded split. An AI insight panel below the map explains the data in natural language. Category Trends shows line charts of demand over the past 4, 12, and 24 weeks for the brand's product categories. Rising and falling trends are highlighted with arrows and percentage changes. Gap Analysis identifies geographic areas where demand exists but supply is low — "45 stores in Indiranagar sell loose dal but no branded dal. Estimated addressable volume: 500kg per week." Each gap has a one-click "Create Bounty for This Gap" button. Seasonal Forecasts show predicted demand shifts for the upcoming 3 months based on historical patterns, festival calendar, and weather trends.

**Analytics and Reports Page:**
Campaign ROI Calculator lets the brand select a campaign and see total spend, total retail sales generated, ROI multiplier, and comparison with industry benchmarks. Geographic Performance shows a sortable table of performance by area with drill-down capability to see individual bounty performance within each area. Conversion Funnel Visualization shows an interactive funnel from Bounty Created through Retailers Matched through Deals Accepted through First Order through Reorder, with drop-off percentages at each stage and AI-generated suggestions for improving each stage. Export functionality allows downloading any report as PDF or CSV.

### 3.3 AI Reasoning Engine — Detailed Design

The AI Reasoning Engine is the central orchestration brain. It is not a separate service but rather a shared logic layer that all Lambda functions invoke when they need coordinated intelligence.

**Store Context Manager:**
This component maintains a comprehensive "memory" for each store in DynamoDB. It includes the complete current inventory state with every item's quantity, velocity, expiry, and margin. It includes the full history of every shelf scan, every voice interaction, and every deal. It includes the AI-inferred demographic profile and category strengths. It includes the shopkeeper's language preference, interaction patterns (do they prefer voice or text? what time do they usually scan?), and past corrections to AI outputs (if the shopkeeper corrected a count, the AI remembers for next time). This context is fetched at the start of every AI processing call and included in the Bedrock prompt so the AI has full awareness.

**Agent Orchestration Logic:**
When a trigger event occurs (shelf photo, scheduled scan, voice input, deal event), the orchestrator determines which agents need to be activated. For a shelf photo upload, it activates Shelf Analyst (primary), then checks results for Liquidation Bot triggers (expiry), Brand Discovery triggers (unbranded items or dead stock), and Trade-Promo triggers (verification). For a scheduled 6-hour check, it activates Trend Hunter only. For a voice input, it first classifies the intent and then routes to the appropriate agent. For a deal acceptance, it activates ONDC Integration and Notification Service.

When multiple agents produce outputs simultaneously (for example, a shelf scan reveals dead stock at the same time the Trend Hunter has a cricket match alert), the orchestrator combines them into a unified response. It follows the Conflict Resolution Logic: (1) Rank by financial impact where a ₹500 expiry risk ranks higher than a ₹200 trend opportunity; (2) Rank by time sensitivity where an event tomorrow ranks higher than an event next week; (3) Rank by confidence level where high-confidence predictions rank higher; (4) Never contradict — if the Shelf Analyst says "reorder chips" but the Trend Hunter says "chip demand is dropping next week," the AI explains both and provides nuanced advice: "You're running low on chips, but demand is expected to drop next week. Order a smaller batch than usual."

**Response Generation:**
Every AI output goes through a Response Generator that produces four outputs: (1) A natural language summary in the user's preferred language for display in the app; (2) Structured data for app UI rendering (card types, color codes, action buttons); (3) A notification payload for push or WhatsApp delivery; (4) Optionally, a voice response via Amazon Polly for users who prefer audio.

---

## 4. Data Architecture

### 4.1 Data Organization Philosophy

All data is organized around two principles: real-time operational data lives in DynamoDB for low-latency access by the mobile app and AI agents, while analytical and aggregated data lives in RDS PostgreSQL for the brand dashboard's intelligence features.

Data flows in one direction: raw data enters through DynamoDB (from shelf scans, voice inputs, deal events), gets processed by AI agents, and analytical summaries are periodically written to RDS for brand consumption. The brand dashboard NEVER reads directly from DynamoDB. It only reads from RDS, which contains only anonymized, aggregated data.

### 4.2 Data Relationships

A Store has many Inventory items. A Store has many ShelfScans. A Store has many Deals. A Store has many Alerts. A Store has many VoiceNotes. A Store has one Power Score (recalculated weekly).

A Brand has many Bounties. A Bounty has many Deals (one per matched retailer who accepts). A Brand has many Sampling Campaigns. A Sampling Campaign has many Store Assignments.

A Deal links one Brand to one Store for one Product. A Deal has many Reorders (tracked over time). A Deal may have Trade-Promo verification data (placement photos and scores).

Market Intelligence records are standalone aggregations not linked to individual stores. They represent anonymized area-level data.

### 4.3 Data Retention and Privacy

Shelf photos are retained for 90 days for velocity comparison, then auto-deleted unless flagged for Trade-Promo verification (those are retained for the deal duration plus 30 days). Voice note audio files are retained for 30 days, then auto-deleted. Transcriptions are retained indefinitely as text (no PII in the audio content itself). Inventory data is retained indefinitely as it feeds historical analysis. Deal data is retained indefinitely for Power Score calculation and brand analytics. Market Intelligence aggregations are retained indefinitely as they are already anonymized.

For brand-facing data, the anonymization layer strips all retailer PII (name, phone, exact address) before any data reaches RDS. The only geographic granularity available to brands is neighborhood-level (e.g., "Jayanagar" not "4th Cross, 2nd Block, Jayanagar"). Store IDs shown to brands are opaque identifiers (e.g., "Store #1247") with no reverse-lookup capability. Retailer identity is revealed only when the retailer explicitly accepts a deal from that specific brand, at which point delivery requires sharing the store address.

---

## 5. AI/ML Architecture

### 5.1 AWS Bedrock Integration Design

The entire AI layer uses a single foundation model: Claude 3.5 Sonnet via Amazon Bedrock. This is a deliberate architectural choice for three reasons: (1) It simplifies the system enormously — one model, one API, one billing relationship; (2) Claude 3.5 Sonnet is multimodal, handling images and text in the same call, which is essential for shelf analysis; (3) It is powerful enough to handle reasoning, language generation, OCR, and multi-language understanding without supplementary models.

Titan Embeddings is used as a complementary service specifically for vector-based semantic search, not for reasoning. It converts product descriptions and store profiles into numerical vectors that enable similarity matching.

Bedrock Knowledge Bases provide RAG capability, ensuring the AI always has access to the latest product catalog, brand bounty database, and store history when generating responses.

### 5.2 Prompt Engineering Strategy

Each agent has a carefully designed system prompt that defines its persona, capabilities, constraints, and output format. All prompts share common principles: always respond in the user's preferred language, never guess when uncertain (ask instead), always explain the reasoning behind recommendations, always quantify financial impact in rupees, and always provide actionable next steps.

**Shelf Analyst System Prompt Design:**
The prompt establishes the AI as an expert retail shelf analyst. It instructs the AI to carefully examine the image, identify every distinct product, count quantities, read any visible expiry dates, assess shelf organization, and compare with the provided previous scan data to calculate velocity. The prompt includes explicit instructions for handling uncertainty: "If you cannot clearly identify a product or read a label, say so and ask the user. Do not guess." It includes the output format specification requiring both structured data (for the app) and a natural language summary (for the user). It includes the store context (previous inventory, active deals, demographic profile) so the AI can provide contextual recommendations.

**Trend Hunter System Prompt Design:**
The prompt establishes the AI as a local market intelligence analyst. It provides the external data (weather, events, news, festivals) and the store's context (location, demographics, historical patterns). It instructs the AI to identify correlations between external signals and likely demand changes, but only for products the store actually sells. It requires confidence levels with every prediction and explicit reasoning chains. It includes guard rails: "Do not generate an alert unless you are at least 65% confident of a demand change greater than 30%. Silence is acceptable when no significant change is predicted."

**Liquidation Bot System Prompt Design:**
The prompt establishes the AI as a pricing optimization specialist. It provides the product details, current stock, expiry date, cost price, selling price, and historical velocity. It instructs the AI to model multiple discount scenarios and recommend the one that maximizes total revenue recovery (not just the one that sells everything — sometimes it's better to sell 80% at a small discount than 100% at a large discount). It requires a clear financial comparison between "doing nothing" and the recommended action. It includes instructions for generating the WhatsApp marketing message in the user's language with appropriate emoji, urgency, and call-to-action.

**Trade-Promo Matching System Prompt Design:**
The prompt establishes the AI as a retail brand strategist. When matching a bounty to retailers, it receives the bounty details and a list of candidate retailers with their profiles. It assesses relevance considering whether the store sells related products, whether the store's customer demographic matches the brand's target, whether the store has proven ability to move products in this category, and whether there are any conflicts with existing collaborations. It generates a relevance score and a personalized pitch for each matched retailer explaining specifically why this deal benefits THEIR store.

### 5.3 Multi-Agent Orchestration

The agents operate in a specific priority hierarchy. The Shelf Analyst runs first on photo input and produces raw data. The Liquidation Bot runs second on any expiry data from the Shelf Analyst. The Trend Hunter runs on its own schedule (every 6 hours) independent of user actions. The Trade-Promo Agent runs third, triggered by opportunities identified by the Shelf Analyst (dead stock, unbranded volume) or by new bounties from brands. The Power Score Engine runs last, weekly, consuming data from all other agents.

When the Orchestrator calls Bedrock, it constructs a master prompt that includes the relevant agent's system prompt, the store's full context from the Store Context Manager, any relevant outputs from previously-run agents in this cycle, and the specific input data (image, voice transcription, or event data). This ensures the AI has complete awareness and can generate responses that account for all factors simultaneously.

### 5.4 Context and Memory Management

The AI's "memory" for each store is managed through a combination of DynamoDB (structured recent data) and Bedrock Knowledge Bases (long-term indexed data).

Short-term memory includes current inventory state, last 7 days of shelf scans, last 30 days of alerts and responses, active deals and their status, and recent voice interactions. This data is fetched from DynamoDB and included directly in the Bedrock prompt.

Long-term memory includes the full product history (what has the store ever sold, seasonality patterns over months), full interaction history (what has the shopkeeper asked, what corrections have they made to AI outputs, what deals have they accepted or rejected), and neighborhood-level patterns (how does this store compare to others in the same area). This data is stored in Bedrock Knowledge Bases and retrieved via RAG when relevant.

The AI uses past corrections to improve. If the shopkeeper corrected a count ("Actually I have 15, not 12"), the AI remembers this for the next scan of the same shelf section and adjusts its confidence thresholds. If the shopkeeper consistently rejects certain types of deals, the AI learns to deprioritize those. If the shopkeeper always acts on expiry alerts but ignores trend alerts, the AI adjusts notification priority accordingly.

---

## 6. API Design

### 6.1 API Organization

All APIs are versioned under /api/v1/ and organized into resource groups. Each group has clear CRUD operations with consistent response formats.

**Auth APIs** handle sending OTP to a phone number, verifying OTP and issuing a JWT token, and refreshing tokens.

**Retailer APIs** handle getting and updating the retailer profile, getting the current Power Score with breakdown, and getting earnings history from brand deals.

**Shelf APIs** handle uploading a shelf photo for analysis (returns a scan ID for polling), getting scan results by ID, listing all scans with pagination and date filters, submitting user corrections to AI analysis, and getting the consolidated velocity report across all shelf sections.

**Voice APIs** handle uploading a voice note for processing, getting processed voice note results, and sending a text chat message to the AI (same processing pipeline but skipping Transcribe).

**Inventory APIs** handle getting the current inventory state (complete or filtered by category), submitting manual stock entries (text-based), getting items expiring within a specified number of days, and getting dead stock items.

**Deal APIs** handle getting personalized available deal cards (for the Deals tab), accepting a deal by ID (triggers ONDC order), dismissing a deal by ID, getting currently active deals, getting deal history, and submitting a shelf photo for Trade-Promo verification of a specific deal.

**Alert APIs** handle getting all alerts with filters for type and date, getting trend alerts specifically, getting expiry alerts specifically, and marking an alert as read.

**Liquidation APIs** handle generating a liquidation offer for a specific expiring item, getting a generated offer's details, and triggering the WhatsApp message send (via Twilio).

**Brand Auth APIs** handle brand registration and login with email/password via Cognito.

**Brand Bounty APIs** handle creating new bounties with full configuration, listing all bounties with status filters, getting bounty details including performance metrics and verification photos, updating bounty parameters, and deactivating bounties.

**Brand Sampling APIs** handle creating sampling campaigns with targeting and instructions, listing campaigns with status filters, getting campaign details including cohort analysis and conversion data, and updating campaign parameters.

**Brand Retailer APIs** handle discovering and filtering retailers with anonymized results, getting anonymized retailer profiles by opaque ID, and getting AI-recommended retailers for the brand.

**Brand Intelligence APIs** handle getting demand heatmap data by category and geography, getting category trend data, getting market gap analysis, and getting seasonal forecast data.

**Brand Analytics APIs** handle getting campaign ROI calculations, getting geographic performance breakdowns, and exporting reports in PDF or CSV.

**ONDC APIs** handle creating logistics orders from accepted deals, getting order tracking status, and receiving ONDC delivery status webhooks.

### 6.2 API Response Format

All API responses follow a consistent structure with a status field (success or error), a data field containing the response payload, a message field with a human-readable description, and a metadata field with pagination info, request ID, and timestamp.

Error responses include an error code, a user-friendly message in the user's language, and a technical detail field for debugging.

### 6.3 Rate Limiting

Retailer APIs are limited to 60 requests per minute per user (sufficient for normal app usage). Brand dashboard APIs are limited to 120 requests per minute per user (higher for data-intensive dashboard operations). Shelf scan uploads are limited to 10 per hour per store (prevents abuse while allowing thorough daily scanning). Voice note uploads are limited to 30 per hour per store.

---

## 7. User Interface Design

### 7.1 Design Principles for Retailer App

**Voice-First, Camera-Second, Text-Third:** Every feature is accessible via voice command or camera input. Text input exists as an option, never a requirement.

**Large Touch Targets:** All tappable elements are minimum 48x48dp with generous spacing. This accounts for the target user operating a phone with one hand while managing their shop with the other.

**High Contrast and Readability:** Text uses high contrast ratios suitable for viewing in bright sunlight (common in Indian shop environments). Font sizes are generous. Important numbers (prices, quantities, margins) are displayed extra-large.

**Color-Coded Cards:** Every alert and deal type has a consistent color: Red for urgent items requiring immediate attention (expiry, spoilage), Yellow for important but not critical items (trend alerts, seasonal reminders), Green for positive opportunities (new deals, margin improvements), Blue for proactive AI insights (unbranded conversion opportunities, brand discovery), White for informational items (weekly reports, Power Score updates), and Purple for sampling opportunities.

**Minimal Navigation Depth:** No screen is more than 2 taps away from the home screen. The most critical actions (scan shelf, talk to AI, view deals) are accessible with 1 tap from anywhere via the bottom navigation.

**Celebration of Savings:** The app prominently displays how much money the AI has saved or earned for the retailer. This reinforces the value proposition and drives continued engagement. Every week, a summary card shows "You saved ₹X this week by following my suggestions."

### 7.2 Design Principles for Brand Dashboard

**Data-Dense but Clean:** The dashboard shows significant amounts of data but uses clear hierarchy, whitespace, and progressive disclosure (click to expand details).

**Map-Centric:** Geography is central to the brand's decision-making. Maps appear on multiple screens: demand heatmaps, retailer discovery, bounty targeting, and campaign performance.

**Actionable Insights:** Every data visualization includes an AI-generated narrative interpretation and a call-to-action button. The dashboard never shows data without suggesting what to do about it.

**Guided Campaign Creation:** Multi-step wizards with AI pre-filled recommendations guide brands through bounty and campaign creation. The AI suggests targeting parameters based on the brand's product category and past performance.

---

## 8. Offline Architecture

### 8.1 Offline Queue Design

The mobile app maintains a local SQLite database that stores queued items when the device is offline. Each queue entry includes the data type (photo, voice note, manual stock entry, deal response), the raw data (image bytes, audio bytes, or structured text), the timestamp of creation, the retry count, and the sync status (pending, uploading, completed, failed).

### 8.2 Offline-Available Features

Taking shelf photos (stored locally with full resolution), recording voice notes (stored locally as audio files), viewing the last 7 days of AI reports and analysis results (cached as JSON in local storage), viewing currently active deals and their details (cached), viewing the current Power Score and breakdown (cached), manual stock entry (stored locally), and reading the last 10 conversation messages in the Voice tab (cached).

### 8.3 Online-Required Features

AI analysis of photos (requires Bedrock), AI processing of voice notes (requires Transcribe and Bedrock), Trend Hunter alerts (requires external data APIs), deal discovery and browsing new available deals (requires server), deal acceptance and ONDC order placement (requires server and ONDC network), WhatsApp message generation and sending (requires Bedrock and Twilio), and real-time Power Score recalculation (requires server).

### 8.4 Sync Behavior

When connectivity is detected, the app checks the offline queue. Items are uploaded in chronological order (oldest first). Photos are compressed before upload to minimize bandwidth (maximum 2MB per image). A small banner at the top of the screen shows "Syncing X items..." with a progress indicator. If an upload fails (timeout, server error), the retry count increments and the app retries with exponential backoff. After 5 failures, the item is marked as failed and the user is prompted to retry manually. As each item syncs successfully, the AI processes it and delivers results. If multiple items sync at once (e.g., 3 shelf photos taken offline over the day), the AI processes them in order and delivers a consolidated update rather than 3 separate notifications.

---

## 9. Security Architecture

### 9.1 Authentication and Authorization

Retailers authenticate via phone OTP through Amazon Cognito. No passwords are stored. Each successful authentication issues a JWT token with a 7-day expiry and automatic refresh. Brand users authenticate via email and password through Amazon Cognito with optional MFA.

All API requests require a valid JWT token in the Authorization header. API Gateway validates the token before routing to any Lambda function. Retailer tokens can only access retailer APIs for their own store ID. Brand tokens can only access brand APIs for their own brand ID. No cross-tenant data access is possible.

### 9.2 Data Encryption

All data in transit is encrypted with TLS 1.3. All data at rest is encrypted with AES-256 via AWS-managed keys. S3 bucket policies enforce encryption and deny any unencrypted uploads. DynamoDB tables have encryption enabled at the table level. RDS instances use encrypted storage volumes.

### 9.3 Privacy Architecture

The Anonymization Layer sits between DynamoDB (which contains retailer PII) and RDS (which serves the brand dashboard). When analytical data is written to RDS, it passes through this layer which strips retailer name, phone number, exact address (keeping only neighborhood-level geography), and replaces the store ID with an opaque identifier. This is not a runtime operation — it happens at data write time, so PII never enters RDS at all.

S3 bucket policies restrict shelf photo access. Only the retailer's own Lambda functions can read their photos. Brand dashboard APIs cannot access S3 shelf photos directly. Trade-Promo verification photos are copied to a separate S3 prefix with restricted access granted only to the specific brand involved in that deal, and only after the retailer has accepted the deal.

### 9.4 API Security

Amazon API Gateway enforces rate limiting per user to prevent abuse. WAF (Web Application Firewall) rules protect against common attacks. Input validation in Lambda functions sanitizes all user inputs. Bedrock prompts include guard rails against prompt injection attacks.

---

## 10. Deployment Architecture

### 10.1 AWS Region

Primary deployment in ap-south-1 (Mumbai) for lowest latency to Indian users and compliance with Indian data residency preferences.

### 10.2 Multi-AZ Design

DynamoDB natively replicates across availability zones. RDS uses a Multi-AZ deployment with automatic failover. S3 natively replicates across availability zones. Lambda functions run across multiple AZs automatically.

### 10.3 CI/CD Pipeline

AWS CodePipeline automates the deployment process. Code is committed to GitHub. CodeBuild runs tests and builds the Lambda deployment packages and React Native APK. CodeDeploy pushes Lambda updates with blue/green deployment. Amplify auto-deploys the brand dashboard from the same repository. The mobile app is distributed via direct APK download (for hackathon) or Google Play Store (for production).

### 10.4 Monitoring and Alerting

Amazon CloudWatch collects logs from all Lambda functions with structured JSON logging for easy searching. Custom CloudWatch metrics track business KPIs: shelf scans per hour, deals accepted per day, API latency percentiles, and Bedrock call success rate. CloudWatch Alarms trigger SNS notifications to the development team for error rate spikes (more than 5% errors in 5 minutes), latency spikes (p99 above 30 seconds), and Bedrock throttling events.

### 10.5 Cost Optimization

Lambda functions use ARM64 (Graviton) processors for 20% cost reduction. S3 lifecycle policies move old shelf photos to Glacier after 90 days. DynamoDB uses on-demand capacity mode so there is no cost for idle capacity. Bedrock calls are optimized by batching where possible (e.g., the 6-hourly Trend Hunter processes multiple stores per invocation rather than one Lambda per store).

---

## 11. Demo Script for Hackathon

### 11.1 Demo Duration
Target 3-4 minutes maximum.

### 11.2 Demo Flow

**Scene 1 — The Problem (30 seconds):**
Open with a brief narrative: "Meet Ramesh. He runs a Kirana store in Bangalore. Every month, he loses ₹15,000 to expired milk, missed cricket-match sales, and razor-thin margins from big brands. That's half his profit — gone. Vyapar-IQ is his AI-powered Chief Merchandising Officer."

**Scene 2 — Shelf Scan (45 seconds):**
Open the Vyapar-IQ app on a phone. The AI greets in Hindi: "Namaste Ramesh sir! Aaj 3 items expire ho rahe hain." Take a photo of a pre-arranged shelf with products. The AI analyzes and shows results: "Lay's selling fast — 8 per day, reorder! Brand X chips — dead stock, 0 sold in 7 days, stop buying. Amul milk expires in 2 days. And I notice you have loose rice — I found a branded deal for you." Point out the color-coded results: green for fast sellers, red for dead stock, orange for expiring.

**Scene 3 — Liquidation Bot (30 seconds):**
Tap the expiry alert for milk. The AI shows: "30 packets expire Feb 16. If you do nothing, you lose ₹250. My suggestion: sell at ₹24 (₹4 discount), broadcast on WhatsApp, you save ₹240." Show the pre-generated WhatsApp message in Hindi with emoji. Tap "Send to WhatsApp" — the phone opens WhatsApp with the message ready.

**Scene 4 — Voice Command (30 seconds):**
Tap the voice button. Speak in Hindi: "Maine aaj 100 ande khareedein, expiry 20 February." The AI responds in Hindi text: "100 eggs logged. Expiry Feb 20. I'll remind you on Feb 18."

**Scene 5 — Deal Card and Margin Boost (45 seconds):**
Switch to the Deals tab. Show a green deal card: "Green Valley Organic Rice — your loose rice makes ₹5/kg. This brand gives you ₹12/kg. Your customer pays ₹47 instead of ₹50. Everyone wins." Swipe right to accept. Show the ONDC delivery confirmation: "Arriving by tomorrow 6 PM."

**Scene 6 — Brand Dashboard (30 seconds):**
Switch to the laptop showing the brand dashboard. Show the demand heatmap: "85% of rice in Jayanagar is unbranded — massive opportunity." Show a bounty performance card: "180 retailers accepted, 95 reordered, 6.4x ROI." Show the visual verification gallery with shelf photos and AI placement scores.

**Scene 7 — The Close (30 seconds):**
"In under 4 minutes, Vyapar-IQ saved Ramesh ₹240 in spoilage, predicted ₹5,000 in cricket-match revenue, tripled his rice profit from ₹5 to ₹12 per kg, and connected him directly with a brand — no middlemen. Multiply this by 12 million stores. That's Vyapar-IQ — India's retail intelligence layer."

### 11.3 Demo Data Preparation

For the hackathon demo, prepare a shelf with clearly labeled products including at least one item with a visible expiry date, one obviously slow-moving item, one brand that represents a Trade-Promo collaboration, and some loose items in bins representing unbranded goods. Pre-load the app with one week of simulated historical data so the AI can show velocity comparisons. Pre-load the brand dashboard with sample bounty data showing realistic performance metrics. Pre-load a weather forecast showing rain and a cricket match event so the Trend Hunter can demonstrate a live alert.

### 11.4 Fallback Plan

If the live AI call to Bedrock takes too long during the demo, have pre-computed results cached locally that the app can display with realistic loading animations. The architecture and AI pipeline remain the same — the cache just ensures a smooth demo experience.
