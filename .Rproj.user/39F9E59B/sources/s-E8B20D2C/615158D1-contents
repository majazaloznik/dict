library(dplyr)

# import dictionary
dict <- read_delim("GoogleDictionaryHistory03.csv", delim = "\t",
                 col_names = FALSE)

# import errors to be updated
err <- read_csv("errors.csv", 
                   col_names = FALSE)

# extract list of words to be removed
err %>%  
  filter(X4 == 0) %>% 
  pull(X2) -> removal

# extract updated definitions with new terms
err %>% 
  filter(!is.na(X5)) -> update

# extract second kind of updates, with original term
err %>% 
  filter(!is.na(X6) & is.na(X5)) -> update2

# clean up data
dict %>% 
  select(term = X3,  def = X4)  %>% 
  distinct() %>% 
  filter(!term %in% removal)  %>% 
  add_row(term = update$X5, def = update$X6) %>% 
  add_row(term = update2$X2, def = update2$X6) %>% 
  arrange(term) -> dict

# manual changes:

dict %>% 
  filter(!term %in% c("x", "adversitate", "e", "enmities", "jago")) %>% 
  mutate(term = ifelse(term == "accoster", "accost", term)) %>% 
  mutate(def = ifelse(term == "accost", 
                      "approach and address (someone) boldly or aggressively.", 
                      def)) %>% 
  mutate(def = ifelse(term == "bespoke", "made for a particular customer or user.", def)) -> dict

 
# export
sink("outfile.txt")
cat(paste0("\\entry{",dict$term, "}{", dict$def, "}", collapse="\n \n")  )
sink()

