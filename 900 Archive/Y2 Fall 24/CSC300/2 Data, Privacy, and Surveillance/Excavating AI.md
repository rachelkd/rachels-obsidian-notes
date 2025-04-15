---
dg-publish: false
tags: [lecture, note, university]
Course Code:
  - "[[CSC300]]"
Week: 
Module:
  - "[[2 - Data, Privacy, and Surveillance]]"
Lecture: 
Chapter: 
Slides/Notes: 
Date: 
Date created: Wed., Dec. 11, 2024, 9:54:03 pm
Date modified: Thu., Dec. 12, 2024, 1:23:12 am
---

# Excavating AI: The Politics of Images in Machine Learning Training Sets

See [[Week 4 - Politics of Data]].

### Summary

The two excerpts delve into the complexities and ethical dilemmas surrounding the categorization and labeling of images within machine learning datasets, particularly in the context of computer vision and AI systems. These datasets, such as ImageNet and UTKFace, rely heavily on categorizing visual data to train AI models, but they also expose deep biases and flawed assumptions that echo historical practices like physiognomy, eugenics, and racial classification systems.

#### 1. **ImageNet’s Labeling Practices**

   ImageNet, a large-scale image dataset, has been used to train machine learning models, but its labeling system is problematic. The labels often simplify or misinterpret the content of images in ways that are not only misleading but also politically charged. For instance, a photograph of a distressed child might be labeled as a “toy,” or a sleeping woman could be labeled as a “snob.” Such labels are indicative of the system’s failure to capture nuanced human experiences, instead offering arbitrary or demeaning classifications. Furthermore, labels based on visual analysis, such as categorizing people as “losers” or “snobs,” reflect problematic assumptions that certain characteristics or behaviors can be deduced purely from appearances. These labels align with outdated ideas from the 19th century, when physiognomy was used to determine moral and criminal traits based on physical features. This raises questions about the legitimacy of assuming that abstract human concepts (e.g., criminality, gender identity) can be visually recognized or quantified.

#### 2. **UTKFace Dataset: Race and Gender Assumptions**

   The UTKFace dataset, which categorizes faces based on age, gender, and race, is another example of problematic image labeling. Its binary gender classification oversimplifies human identity, assuming that a person’s gender can be accurately deduced from their appearance. The race classification system is similarly limited and echoes problematic racial categorization systems from the past, such as those used during apartheid in South Africa. This categorization doesn’t account for the fluidity of identity and reinforces racial and gender stereotypes. The dataset’s creators position these categories as tools for improving AI accuracy, but the underlying assumptions about identity, as well as the reduction of complex, multifaceted human beings to a few physical traits, highlight the inherent biases in the technology.

#### 3. **IBM’s “Diversity In Faces” and Craniometry**

   IBM’s “Diversity in Faces” dataset was designed to address criticism that facial recognition systems struggled with darker skin tones. The dataset attempts to achieve statistical parity by considering factors like skin tone, age, and facial structure. However, it introduces new concerns by incorporating facial symmetry and skull shapes to improve classification accuracy. This approach recalls 19th-century craniometry, a pseudoscientific practice that used skull measurements to claim racial superiority. The inclusion of craniofacial features as a basis for classification raises serious ethical questions, as it can perpetuate the same harmful stereotypes that historical practices sought to legitimize. Furthermore, the binary gender classification in this dataset continues to overlook the complexities of gender identity.

#### 4. **The Politics of Labeling and Categorization**

   The key issue across these datasets is that categorization is inherently political. The process of labeling images—whether they be of faces, objects, or actions—reflects cultural, historical, and political assumptions about identity, race, gender, and behavior. The categorization of people into fixed groups based on visual characteristics not only oversimplifies the diversity of human experience but also risks perpetuating harmful stereotypes and biases. The attempt to “correct” these biases by diversifying datasets doesn’t address the deeper issue that the entire process of categorization itself is fraught with problematic assumptions. The creation of datasets, like ImageNet or IBM’s “Diversity in Faces,” is not neutral; it is a political act that shapes how AI systems interact with and understand the world.

#### 5. **Missing And Disappearing Datasets**

   Another significant issue is the disappearance of datasets like MS-CELEB and the Duke Multi-Target, Multi-Camera dataset. These datasets were taken offline due to privacy concerns and ethical violations. However, their removal doesn’t erase their impact. These datasets have already been used in AI systems that influence public life, including hiring practices, security, and surveillance. The disappearance of these datasets highlights the difficulty in understanding and auditing AI systems once they are deployed. Researchers are unable to trace how biases and assumptions embedded in the datasets might have affected the outcomes of AI systems, raising concerns about the lack of accountability in AI development.

#### 6. **Conclusion: The Politics of Data Collection**

   The final takeaway is that the creation and use of datasets for machine learning and AI is deeply political. Decisions about which images are included, how they are labeled, and what categories are created all reflect societal norms, biases, and historical contexts. As AI systems become more integrated into everyday life, it becomes increasingly important to examine the assumptions embedded in these systems and ensure that they do not perpetuate harm. Data collection, labeling, and categorization are not neutral acts but are shaped by cultural, political, and historical forces. The normalization of certain representations and the exclusion of others contributes to how AI technologies interact with and shape the world.

In summary, the practice of labeling images for AI systems carries significant political, ethical, and social implications. These datasets, while essential for training AI models, are not objective or neutral but are deeply embedded with the biases, assumptions, and stereotypes of the creators. As AI continues to be deployed in various aspects of life, it is crucial to critically examine the datasets that power these systems and to be aware of the ways in which they shape our understanding of identity, race, gender, and human behavior.
