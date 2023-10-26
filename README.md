<!-- Improved compatibility of back to top link: See: https://github.com/othneildrew/Best-README-Template/pull/73 -->
<a name="readme-top"></a>
<!--
*** Thanks for checking out the Best-README-Template. If you have a suggestion
*** that would make this better, please fork the repo and create a pull request
*** or simply open an issue with the tag "enhancement".
*** Don't forget to give the project a star!
*** Thanks again! Now go create something AMAZING! :D
-->



<!-- PROJECT SHIELDS -->
<!--
*** I'm using markdown "reference style" links for readability.
*** Reference links are enclosed in brackets [ ] instead of parentheses ( ).
*** See the bottom of this document for the declaration of the reference variables
*** for contributors-url, forks-url, etc. This is an optional, concise syntax you may use.
*** https://www.markdownguide.org/basic-syntax/#reference-style-links
-->




<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#installation">Installation</a></li>
      </ul>
    </li>
    <li><a href="#usage">Usage</a></li>
    <li><a href="#roadmap">Roadmap</a></li>
    <li><a href="#walkthrough">Walkthough</a></li>
        <ul>
        <li><a href="#### 1. Match-making Tool">Match-making Tool</a></li>
        <li><a href="#### 2. Bulk Email Sending Tool">Bulk Email Sending Tool</a></li>
        </ul>
    <li><a href="#contributing">Contributing</a></li>
    <li><a href="#license">License</a></li>
    <li><a href="#contact">Contact</a></li>
    <li><a href="#acknowledgments">Acknowledgments</a></li>
  </ol>
</details>



<!-- ABOUT THE PROJECT -->
<!-- ABOUT THE PROJECT -->
## About The Project

<!-- [![Product Name Screen Shot][product-screenshot]](https://example.com) -->


This project provides two tools for project Tutor Program for International Students ([TPIS](https://mp.weixin.qq.com/s/FxYtpT5HCkXFLltBPmtTJg)) from Students' International Communication Association ([SICA](https://newsen.pku.edu.cn/news_events/news/global/13656.html)), Peking University, to match local and international students with **similar expectations** optimally, and to send emails to partners with each others' `WeChat ID` from the excel file automatically.

<p align="right">(<a href="#readme-top">back to top</a>)</p>



### Built With

- Python
- OpenAI, embedding model

<p align="right">(<a href="#readme-top">back to top</a>)</p>


<!-- GETTING STARTED -->
## Getting Started

To get a local copy up and running follow these simple example steps.

### Prerequisites
1. Prepare Pinecone and OpenAI API key:
    - Pinecone API key can be obtained [here](https://app.pinecone.io/).
    - OpenAI API key can be obtained [here](https://platform.openai.com/account/api-keys).
2. To export the Pinecone and OpenAI API key to system environment
   ``` bash
   export PINECONE_API_KEY="your_pinecone_api_key"
   export OPENAI_API_KEY="your_openai_api_key"
   ```
   now in Python use
   ``` python
   import os
   os.environ["PINECONE_API_KEY"]
   os.environ["OPENAI_API_KEY"]
   ```
   to check if you have them exported to system environment, if `KeyError`, then restart the terminal upon completion (and your IDE if you are using one).
### Installation
1. clone this repo to your local machine
    ```bash
    git clone https://github.com/madeyexz/markdown-file-query.git 
    ```
2. Install the dependencies
    ``` bash
    pip install pinecone langchain tqdm
    ```


<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- USAGE EXAMPLES -->
## Usage

Use this space to show useful examples of how a project can be used. Additional screenshots, code examples and demos work well in this space. You may also link to more resources.

_For more examples, please refer to the [Documentation](https://example.com)_

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- ROADMAP -->
## Roadmap

- [ ] Feature 1
- [ ] Feature 2
- [ ] Feature 3
    - [ ] Nested Feature

See the [open issues](https://github.com/github_username/repo_name/issues) for a full list of proposed features (and known issues).

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Walkthrough
### 1. Match-making Tool
#### Goal
> Output an easy-to-read excel file with each matching groups
- match local and international students with **similar expectations** optimally.
- add a column describing the partner set number, such as `1`, `2`, `3`
- rearrange the order of the students in the excel file according to the matching result, color the sets alternatively for better excel visualization.

#### 1. Excel pre-processing
- translate the entire `expectation_column` into English
- the number of local students must equal international students.

#### 2. Profile creating
- create a `Person()` class object with all datas collected in the excel file as initial variables, along with vector datas like `id` and `coordination`, 

#### 3. Embedding
- using OpenAI API to embed the `expectation` column into a $N$-dimension vector, $(N\gt 1000)$.
  > On a second thought, will $>1000$ be too much? Maybe we can try a smaller dimension.

#### 4. Save the embedding vectors into a vector database
- decide which VDB to use
- include meta vector datas such as `id` of the `Person()`

#### 5. Query the vector database/ Compare the vectors

Here we have propse two ways to do this. [Greedy algorithm](https://en.wikipedia.org/wiki/Greedy_algorithm) and [Maximum Weight Biparate Matching](https://web.ntnu.edu.tw/~algo/Matching.html). We understand that greedy algorithm only optimizes match-making locally, so we will use maximum weight biparate matching as well.

##### Greedy Algorithm
Easiset way to do this is to use a greedy algorithm, which is to match the nearest `local` student with the nearest `international` student, and remove both of them from the database or add them to the don't match list.


##### Maximum Weight Biparate Matching
The algorithm is to find the maximum weight matching between two sets of vertices `{locals}`, `{inernationals}`.

The weights are the cosine similarity of the `expectation` vectors between a local student and an international student. According to the mechanisms of the embedding algorithm, the more similar the `expectation` are, the closer the vectors are, and the heavier the weight is.

Each alrorithm will output a dictionary of matching pairs, containing the `{id_local, id_international}`.

#### 6. Output the result
From the previous output, we will rearrange the excel sheet, and split them into groups.


### 2. Bulk Email Sending Tool
#### Goal
- send emails to partners with each others' `WeChat ID` from the excel file
- an easily customizable email template


<!-- CONTRIBUTING -->
## Contributing

Contributions are what make the open source community such an amazing place to learn, inspire, and create. Any contributions you make are **greatly appreciated**.

If you have a suggestion that would make this better, please fork the repo and create a pull request. You can also simply open an issue with the tag "enhancement".
Don't forget to give the project a star! Thanks again!

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- LICENSE -->
## License

Distributed under the GNU Affero General Public License v3.0. See `LICENSE.txt` for more information.

<p align="right">(<a href="#readme-top">back to top</a>)</p>


<!-- CONTACT -->
## Contact

- Email me: [ian.xiao@stu.pku.edu.cn](emailto:ian.xiao@stu.pku.edu.cn)

- Project Link: [PKU_SICA_TPIS_tool](https://github.com/madeyexz/PKU_SICA_TPIS_tool)

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- ACKNOWLEDGMENTS -->
## Acknowledgments

* [演算法筆記-NTNU @algorithm](https://web.ntnu.edu.tw/~algo/Matching.html)


<p align="right">(<a href="#readme-top">back to top</a>)</p>