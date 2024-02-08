# National-Housing- Data-Cleaning-In-SQL
![house](https://www.houseplans.net/uploads/plans/25042/elevations/73665-1200.jpg?v=030723155928)

## Overview

This repository contains SQL queries for cleaning and preprocessing the National Housing dataset. The data cleaning process includes standardizing date formats, populating missing property address data, breaking out address components into individual columns, modifying categorical values, removing duplicate records, and deleting unused columns.

## Data Cleaning Steps

### 1. Standardize Date Format

```sql
-- SQL Query to standardize date format
SELECT SaleDate, Convert(Date, SaleDate)
FROM PortfolioProject..NationalHousing
-- ... (additional queries for date standardization)
### 2.Populate Property Address Data

  -- SQL Query to populate missing property address data
  SELECT a.ParcelID, a.PropertyAddress, ISNULL(a.PropertyAddress, b.PropertyAddress) AS UpdatedAddress
  FROM PortfolioProject..NationalHousing a
  JOIN PortfolioProject..NationalHousing b
  ON a.ParcelID = b.ParcelID AND a.UniqueID <> b.UniqueID
  WHERE a.PropertyAddress IS NULL
  -- ... (additional queries for address population)
### 3. Breaking Out Address Into Individual Coulumns
-- SQL Query to break out address into individual columns
SELECT
  SUBSTRING(PropertyAddress, 1, CHARINDEX(',', PropertyAddress) - 1) AS Address,
  SUBSTRING(PropertyAddress, CHARINDEX(',', PropertyAddress) + 1, LEN(PropertyAddress)) AS City
FROM PortfolioProject..NationalHousing
-- ... (additional queries for address breakdown)
### 4. Modifying 'Y' and 'N' to 'Yes' and 'No' in 'SoldAsVacant' Column
-- SQL Query to modify values in 'SoldAsVacant' column
UPDATE PortfolioProject..NationalHousing
SET SoldAsVacant = CASE
                     WHEN SoldAsVacant = 'Y' THEN 'Yes'
                     WHEN SoldAsVacant = 'N' THEN 'No'
                     ELSE SoldAsVacant
                   END
-- ... (additional queries for value modification)
### 5. Remove Duplicates
-- SQL Query to remove duplicate records
WITH RowNumCTE AS (
  SELECT *,
         ROW_NUMBER() OVER (
           PARTITION BY ParcelID, PropertyAddress, SalePrice, SaleDate, LegalReference
           ORDER BY UniqueID
         ) AS row_num
  FROM PortfolioProject..NationalHousing
)
SELECT *
FROM RowNumCTE
WHERE row_num > 1
-- ... (additional queries for duplicate removal)
### 6. Delete Unused Columns
-- SQL Query to delete unused columns
ALTER TABLE PortfolioProject..NationalHousing
DROP COLUMN PropertyAddress, SaleDate, OwnerAddress, TaxDistrict
-- ... (additional queries for column deletion)
How to Run
Execute each SQL query in the provided sequence to perform the data cleaning operations.
Verify the changes in the dataset after running the queries.
Feel free to modify the queries based on your specific requirements.

This README provides a structured overview of the project, details each data cleaning step, and includes instructions on how to run the SQL queries. 

