# Magic-Bricks-Property-Recommendation

> **Business question:** Which property segments and locations should MagicBricks target for premium listings and advertising to maximise revenue?

A Power BI dashboard built on MagicBricks listing data that shows where commission revenue comes from and which cities and segments deserve premium listings and ads.

---

## Dashboard

### Page 1: Property Segmentation
![Property Segmentation](Dashboard_Images/Dashboard_Page1_Property_Segmentation.png)

### Page 2: Location Targeting
![Location Targeting](Dashboard_Images/Dashboard_Page2_Location_Targeting.png)

---

## 1. Problem statement
MagicBricks earns money as commission on listings, but premium listing and ad budgets are limited. Without a clear view of where commission comes from, spend gets spread evenly across all cities. This dashboard shows where revenue and demand are both high, so premium products and ads go where they earn the most.

## 2. Dataset
| Item | Detail |
|---|---|
| File | `Dataset/Magic Bricks Dataset.xlsx` |
| Rows | 1,530 listings |
| Period | 02 Jan 2023 to 03 Dec 2025 |
| Cities | 10 (60 localities) |
| Seller types | Builder 542, Owner 496, Broker 492 |
| Transaction | New 797, Resale 733 |

**Columns:** `Listing_iD`, `Location`, `Locality`, `BHK`, `Size`, `Price`, `Bathroom`, `Balcony`, `Furnished`, `Transaction`, `Seller_Type`, `Listing_Date`, `Month`, `Price Per SQFT`, `Commission`

**Data notes**
- `Furnished` has two spellings of one value (`Semi furnished`, `Semi Furnished`). They are merged in the dashboard.
- Owner listings have `Commission = 0`, so they show as an upsell pool and not as revenue.

## 3. Three dimensions behind the analysis
| Dimension | Meaning |
|---|---|
| Total Revenue | How much money flows in the city |
| Total Listings | How active demand is |
| Avg Revenue per listing | Value of each listing |

**Logic:** High Revenue + High Demand = **Vacuum** (busy, rich, no premium service layer) = **Opportunity to serve** (premium listings, ads, service layer).
Analogy: a busy highway full of rich drivers with no petrol pump.

## 4. City tiers
| Tier | Rule (Total Revenue) | Action | Cities |
|---|---|---|---|
| Premium push | 70 Cr or more | Premium listings, full ad push | Mumbai, Delhi_NCR |
| Selective ads | 40 to under 70 Cr | Targeted ads, top segments only | Pune, Bangalore, Hyderabad |
| Maintain | Under 40 Cr | Standard listings, no premium budget | Chennai, Kolkata, Jaipur, Ahmedabad, Lucknow |

## 5. Dashboard contents
**Page 1: Property Segmentation**
1. KPI cards: Total Revenue (446 Cr), Mumbai + Delhi NCR Revenue (174.8 Cr, 39%), 2BHK + 3BHK Revenue (301 Cr, 67%), Total Listing With No Revenue / Owners (496)
2. Total Revenue by BHK and Transaction (Cr)
3. Average Revenue per Listings (Lac)
4. City x Furnished Revenue Heatmap (Cr)
5. City x Seller Type Revenue Heatmap (Cr)

**Page 2: Location Targeting**
1. Total Revenue by City (Cr)
2. Average Revenue by Locality (Cr), top localities
3. City x BHK revenue heatmap (Cr)
4. Targeting priority by city (Tier, Total Revenue, Total_Revenue_per_listing, Free_Owner_Count, Total Listings)

## 6. Key findings
- Total commission: **Rs. 446 Cr**
- Mumbai (98.8 Cr) + Delhi NCR (76.0 Cr) = **39%** of revenue
- Avg commission per listing: Mumbai **64.1 L**, Delhi NCR 41.1 L, Lucknow 13.0 L
- 3BHK Resale is the biggest segment (89.3 Cr); 2BHK + 3BHK = 67% of revenue
- 4BHK Resale earns the most per listing among volume segments
- Mumbai Builder listings earn 59.9 Cr vs Broker 38.9 Cr; Owner earns 0 in every city
- Top localities: Powai (21.8 Cr), Bandra (17.8 Cr), Borivali (17.3 Cr), all in Mumbai
- 496 free owner listings; 41 in Mumbai and 50 in Delhi NCR can be upsold

## 7. Recommendations
1. **Premium push:** Mumbai and Delhi NCR first, starting with Powai, Bandra, Borivali, Thane, Gurgaon, Rohini and Saket.
2. **Segments:** feature 3BHK Resale and 2BHK; promote 4BHK Resale for higher payout per listing.
3. **Selective ads:** Pune, Bangalore, Hyderabad, mainly 3BHK.
4. **Maintain:** no premium ad budget in the other five cities.
5. **Upsell:** convert free owners in Mumbai and Delhi NCR to paid premium listings.

## 8. Power BI measures
```DAX
Total Revenue = DIVIDE(SUM('Magic Bricks'[Commission]), 10000000)

Total Listings = COUNT('Magic Bricks'[Listing_iD])

Total_Revenue_per_listing =
DIVIDE(SUM('Magic Bricks'[Commission]), [Total Listings], 0) / 100000

Free_Owner_Count =
CALCULATE([Total Listings], 'Magic Bricks'[Seller_Type] = "Owner")

Tier =
VAR Rev = [Total Revenue]
RETURN
SWITCH(TRUE(),
    Rev >= 70, "Premium push",
    Rev >= 40, "Selective ads",
    "Maintain")
```
Change the table name `'Magic Bricks'` to match your model.

## 9. Repository structure
```
Magic-Bricks-Property-Recommendation/
├── PBIX/                  Power BI dashboard file (.pbix)
├── Dataset/               Magic Bricks Dataset.xlsx
├── BRD/                   MagicBricks_Dashboard_BRD.pdf
├── Dashboard_Images/      Dashboard page screenshots
└── README.md
```

## 10. How to use
1. Download the repository.
2. Open the `.pbix` file in Power BI Desktop.
3. If the data path breaks, point the source to `Dataset/Magic Bricks Dataset.xlsx`.
4. Use the tier table on Page 2 to decide where to run premium listings and ads.

## 11. Business requirements
Full requirements, scope, KPIs, risks and acceptance criteria are in `BRD/MagicBricks_Dashboard_BRD.pdf`.
