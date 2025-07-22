2024-08-28 11:29

Status:

Tags: #PackCalculator

# Pack Template Notes

Pack templates follow this structure: 

	{
		"uncommon" : {
		"times_repeated" : 3,
		"rng_table" : {
			"category": ["uncommon", "uncommon_mdfc"],
			"chance": ["80.0", "20.0"]
		}
	}

The "chance" column refers to the percent chance of rolling a specific category. The packs are calculated by rolling an RNG number between 1-100. If this number is smaller than the first category, then it is selected. Otherwise, subtract the chance from the RNG and check the next number. Repeat until a category is selected.
## References
