> Path file-a: *app/src/main/java/com/strukovnasamobor/samobornt/services/DBConnection.kt*

    fun searchNames(tableName: String, keyword: String): Cursor{
            return DATABASE!!.rawQuery("SELECT * FROM $tableName WHERE lower(Name) LIKE lower('%$keyword%')", null)
        }
___
> Path file-a: *app/src/main/java/com/strukovnasamobor/samobornt/MapboxActivity.kt*


    /**
         * @param[locale] denotes which table to use when searching. If it is "hr" then the Croatian language table will be used, otherwise the English language table will be used.
         * @param[keyword] text from search, used to look for names in the database(s).
         * @return data from locations which have names similar to the [keyword].
         */
        fun searchDatabaseByLocationName(locale: String, keyword: String): MutableList<Map<String, String>> {
            val locationList: MutableList<Map<String, String>> = mutableListOf()
            val correctTable = if (locale == "hr") TABLE_NAME_HRV else TABLE_NAME_ENG
            val cursor: Cursor = connection.searchNames(correctTable, keyword)
            if (cursor.moveToFirst()) {
                while (!cursor.isAfterLast) {
    
                    val locationName = cursor.getString(cursor.getColumnIndex(C_NAME))
                    val shortDescription = cursor.getString(cursor.getColumnIndex(C_SHORT_DESCRIPTION))
                    val locationId = cursor.getString(cursor.getColumnIndex(C_ID))
                    val mainImage = cursor.getString(cursor.getColumnIndex(C_MAIN_IMAGE))
    
                    val newMap: Map<String, String> = mapOf(
                        "locationName" to locationName,
                        "shortDescription" to shortDescription,
                        "locationId" to locationId,
                        "mainImage" to mainImage
                    )
    
                    locationList.add(newMap)
                    cursor.moveToNext()
                }
            }
            return locationList
        }
___

