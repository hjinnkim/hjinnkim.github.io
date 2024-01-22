---
title: "[Video] DORAEMONGPT : TOWARD UNDERSTANDING DYNAMIC SCENES WITH LARGE LANGUAGE MODELS"
date: 2024-01-22 + 0830
categories: [Video, LLM]
tags: [video, llm]		# TAG는 반드시 소문자로 이루어져야함!
pin: false
math: true
---

# DORAEMONGPT : TOWARD UNDERSTANDING DYNAMIC SCENES WITH LARGE LANGUAGE MODELS

오늘 본 논문은 arXiv "DORAEMONGPT : TOWARD UNDERSTANDING DYNAMIC SCENES WITH LARGE LANGUAGE MODELS"이다. [(https://github.com/z-x-yang/doraemongpt)](https://github.com/z-x-yang/doraemongpt) 

Dynamic Video를 이해하고, Video with a question/task를 효과적으로 풀기 위해 LLM 및 여러 video task 모델들을 합쳤다. LLM으로 효과적으로 querying하기 위해 data를 어떻게 encoding 하고, 동시에 Monte Carlo Tree Search (MCTS)기반 LLM-driven planner를 제안하여, 여러 tool을 이용하여 planning space를 효과적이고 효율적으로 탐색할 수 있도록 했다.

## Abstract

- 계속 변화하고 시각적으로 집약적인 real-world scenario를 반영한 포괄적인 LLM-driven dynamic video understanding system인 Doraemon-GPT를 제안한다.
- Doraemon-GPT는 question/task와 입력 비디오를 받았을 때, 입력 비디오와 다양한 정보를 task-related 특징을 가진 symbolic memory로 변환한다. 이를 통해 sub-task tools로 spatial - temporal querying & reasoning을 가능하게 한다.
- 특수한 분야에 대해서는 LLM이 제한된 정보를 가지고 있기 때문에, 외부 정보를 다룰 수 있는 plug-and-play tools를 도입하고, task를 서로 다른 분야에서 다룬다.
- 마지막으로 Monte Carlo Tree Search (MCTS) 기반 LLM-driven planner를 도입하여, 다양한 tools를 사용하기 위한 large planning space를 효과적으로 탐색한다. 이 planner는 결과 보상을 역전파하여 가능성있는 해답을 찾고, 다양한 해답이 향상된 최종 해답으로 요약된다. 

## Framework Comparison

<iframe src="https://unistackr0-my.sharepoint.com/personal/hyeonjin_kim_unist_ac_kr/_layouts/15/embed.aspx?UniqueId=137406bb-459e-44db-a3ad-a1a04b88ecfa" width="640" height="360" frameborder="0" scrolling="no" allowfullscreen title="3-1.png"></iframe>

## Task-related Symbolic Memory (TSM) Construction

TSM을 구축하기 위해 가장 직관적인 in-context learning을 사용하여 Quesiton Q를 기반으로 TSM의 task type을 선정하였다. (GPT가 우리 질문을 기반으로 (내부 정보를 이용하여) 적절한 답을 도출하는 것을 뜻한다. 그냥 GPT와 같은 LLM의 내부 학습 정보를 이용하여 질문에 가장 적절한 답변을 추론하는 과정이라고 생각하면 된다. "learning"이라는 말이 붙지만 그냥 pretrained-LLM을 이용하여 task-realated 답변을 얻기 위해 task를 input-text에 포함시키겠다는 뜻) 이를 위해 TSM의 각 타입의 text descriptiond을 LLM-driven planner를 위한 context에 포함시켰다. 이 context에서 task-related attributes를 추출하고 SQL 형태로 저장하여 symbolic launguage에 의해 사용된다.

Video task를 분류하는 전형화된 기준이 없기 때문에, 저자는 두 memory의 타입을 디자인하기 위해 video representation 연구에서 자주 적용되는 **spatial-temporal decoupling** 특성을 차용하였다.

- Space-dominant memory는 특정 targets (persons or animals)이나 공간적 관계와 관련된 질문을 다루기 위해 사용된다. 객체를 감지하고 추적하기 위해 multi-object tracking methods를 사용한다. 각 객체는 1) unique ID, 2) semantic category, 3) 위치 추정을 위한 trajectory & segmentation, 4) text-based groudning과 action classification을 위한 Visual Language understanding 모델에서 추출된 apperance description 을 attributes로 가지고 있다.
- Time-dominant memory는 video의 질문에서 temporal-related information을 구축하는데 집중한다. Time-dominant memory의 attributes는 1) timestamp, 2) audio content (by speech recognition model), 3) optical content (by Optical Character Recognition), 4) frame-level captioning, 5) 유사하고 연속적인 frame-level 결과를 추론한 clip-level captioning 를 포함한다.

<iframe src="https://unistackr0-my.sharepoint.com/personal/hyeonjin_kim_unist_ac_kr/_layouts/15/embed.aspx?UniqueId=e04f917a-6de2-4b1b-9690-41f7917f3254" width="640" height="360" frameborder="0" scrolling="no" allowfullscreen title="3-2.png"></iframe>

## Sub-task Tools

LLM-driven agents들이 배우지 않은 정보를 분석하기 위해 in-context learning을 활용하거나 symbolic sentence를 생성하여 memory에 접근하지만, 이 방식은 context의 길이를 매우 증가시켜 추론 과정에서 중요한 정보를 누락하거나 context의 너무 많은 정보로 인해 영향을 받을 수 있다. 

저자들은 연속적인 sub-task 질문들을 답하는 방식으로 TSM으로부터 정보를 추출하는 일련의 sub-task tools를 제공한다. LLM-driven planner는 "sub-task description, tool name, tool inputs"를 묘사하는 in-context description을 통해 각 sub-task tool을 학습한다. 

sub-task tool API를 호출하기 위해 DoraemonGPT는 LLM에 의해 생성된 command를 "Action: <tool name>. Input: <video_name>#<sub_question>.." 형태로 분해한다.

위에서 정의된 두 가지 형태의 TSM을 조합하기 위해, 저자들은 각 sub-task description에 따라 sub-task tools를 설계하고, 각 sub-questions들을 풀기 위해 다음을 포함한다.

<iframe src="https://unistackr0-my.sharepoint.com/personal/hyeonjin_kim_unist_ac_kr/_layouts/15/embed.aspx?UniqueId=9688e38c-0d4d-4178-b079-03780ed7b78f" width="640" height="80" frameborder="0" scrolling="no" allowfullscreen title="3-3.png"></iframe>

<iframe src="https://unistackr0-my.sharepoint.com/personal/hyeonjin_kim_unist_ac_kr/_layouts/15/embed.aspx?UniqueId=a872726a-2ddb-44c1-a63a-72592b41fac0" width="640" height="80" frameborder="0" scrolling="no" allowfullscreen title="3-4.png"></iframe>

각 sub-task tool function 들은 개별적인 LLM-driven agent이며, TSM을 query하기 위해 SQL 문장을 생성하고, 주어진 sub-task question에 답한다. 서로 다른 sub-task agents는 그들의 목적에 맞는 서로 다른 in-context 예시들을 가진다. 한 sub-question이 둘 혹은 더 많은 sub-tools에 적합할 수 있다.

## Knowledge Tools and Others

복잡한 문제가 주어졌을 때, LLM-driven agents들은 LLM 훈련시 학습한 함축적인 정보와 video understanding으로만으로 정확한 판단을 하는데 실패하곤 한다. DoraemonGPT는 LLM이 입력 비디오/질문의 전문화된 정보를 이해하는 것을 도와줄 수 있는 외부 정보원 (external knowledge source)들을 지원한다. DoraemonGPT는 plug-and-play 방식의 개별적인 knowledge tools를 사용하여 외부 정보원을 사용할 수 있다.

Knowledge tools는 1) 주어진 knowledge source를 묘사하기 위한 in-context knowlege description, 2) question answering을 통한 source로부터의 information query를 수행하는 API function으로 이루어져 있다.

다양한 knowledge를 다루기 위해 저자들은 3가지 유형의 API function을 고려했다.

1) symbolic knowlege는 SQL tables이나 Excel과 같은 structured format의 정보를 다루는 *symbolic knowledge*. 이 API function은 sub-task tools와 같은 symbolic question-answering sub-agent 이다.

2) 연구 논문이나 교재와 같은 natural language를 다루는 *textual knowledge*. 이 API function은 text embedding & searching [[link]](https://openai.com/blog/new-and-improved-embedding-model)을 기반으로 구축된다.

3) 인터넷의 정보를 다루는 *web knowledge*. 이 API function은 Google, Bing과 같은 검색 엔진이다. 

이와 더불어 DoraemonGPT는 video editing이나 inpainting과 같은 더욱 전문화된 vision task를 돕기 위한 general utility tools을 지원한다.

## Monte Carlo Tree Search (MCTS) Planner

기존의 LLM-driven planner들은 주어진 질문 Q를 action/sub-task sequence로 분해하고 step-by-step으로 해결하였다. 이와 같은 방식은 최종 답안을 도출하기까지 연쇄적인 action nodes를 생성하는 greedy search 방식으로 생각할 수 있다.

저자들은 dynamic video tasks를 풀이하는 large planning space를 (자료구조의) tree structure로 고려하여 하나의 시도는 정확한 결과를 얻지 못하거나, 더 나은 답안이 존재할 수 있다고 생각하였다. 이 planning space를 효율적으로 탐색하기 위해 저자들은 대규모 tree 구조에서 실용성을 보인 *MCTS*를 탑재한 tree-search-like planner를 제안하였다.

저자들은 먼저 입력 질문 Q를 root node $v_0$로, action / tool call을 non-root node로 정의하였다. 이 경우 action sequence를 root node로부터 leaf node로 가는 path로 볼 수 있다. 

이 MCTS에서 non-root node는 <thought, action, action input, observation> 형태의 ReAct-style step이며, leaf-node는 final answer를 추가로 포함하고 있다. Planner는 다음의 과정을 N번 반복하고, N개의 풀이를 제안한다.

### Node Selection

각 iteration는 새로운 풀이를 계획할 수 있는 확장 가능한 node를 선택하는 것으로 시작한다. 첫 iteration에서는 root node $v_0$만이 선택 가능하다. 이어지는 iteration들에서는 각 node들의 $P(v_i)=Softmax(R_i), \text{where } R_i=\text{reward value of }v_i$로 구현된 sampling probability를 기반으로 무작위로 선택한다. 초기에는 각 node들의 reward value가 0으로 초기화되며, 이후 *Reward Back-propagation* 단계에서 update된다. 높은 reward를 가진 node가 더 많이 선택된다.

### Branch Expansion

선택된 확장 가능한 node에는 새로운 branch를 생성하기 위해 child node가 추가된다. 이전의 child node들과 다른 새로운 tool을 생성하는 것에 LLM을 사용하기 위해, LLM prompt에 historical tool action을 추가하고, LLM이 다른 선택을 하도록 지시한다. 이와 같은 in-context prompt는 새로운 최종 답안을 향한 이어지는 chain execution에서 제거된다. 

### Chain Execution 

새로운 branch를 확장한 뒤, 새로운 해답을 생성하기 위해 step-wise LLM-driven planner를 사용하였다. Execution process는 연쇄적인 tool calls의 step/nodes로 이루어져 있으며, 최종 답안을 얻거나 execution error이 발생하면 종료된다.

### Reward Back-propagation

Leaf/outcome node $v_l$을 획득한 뒤, 해당 node의 reward를 조상 nodes (ancestor nodes)들을 통해 $v_0$까지 점진적으로 전파한다. 여기서 두 가지 형태의 reward가 존재한다.

1) Failure: planner가 tool call 실패, 부정확한 result format 등의 unexpected result를 생성한 상황이다. Reward $R_{v_l}$는 -1과 같은 negative value로 설정된다.

2) Non-failure: planner가 failure resuls에 포함되지 않는 결과를 성공적으로 생성한 상황이다. 하지만 ground truth만큼 정확한 결과인지는 확실하지 않다. $R_{v_l}$은 1과 같은 positive value로 설정된다.

"Lost in the middle: How language models use long contexts" 논문에 따라, LLM에 의해 생성된 결과는 시작 context (initial prompts)와 마지막 context (final prompts)와 더욱 연관되어 있다. 저자들은 $v_l$에 가까운 nodes들에 더 큰 reward가 주어져야 한다고 생각했다. 따라서 Backpropagation function은 $R_{v_i}\leftarrow R_{v_i}+R_{v_l}e^{\beta(1-d(v_i, v_l))}$, $d(v_i, v_l)$는 $v_i, v_l$ 사이 node distance, $\beta$는 reward decay rate를 조절하는 hyperparameter이다. 

모든 MCTS 과정을 거친 뒤, planner는 최대 N non-failure answers를 생성하고, LLM을 사용하여 모든 답을 요약하여 의미있는 해답을 생성할 수 있다. single/multiple choice question에서는 voting process를 통해서도 최종 해답을 결정할 수 있다.

## Example

<iframe src="https://unistackr0-my.sharepoint.com/personal/hyeonjin_kim_unist_ac_kr/_layouts/15/embed.aspx?UniqueId=99bb688d-31e8-4c75-87b3-bee5f98c876a" width="640" height="360" frameborder="0" scrolling="no" allowfullscreen title="4-1.png"></iframe>

## 마무리

Video 연구에서 foundation / large model을 활용해볼 수 있을까 하여 읽었는데, 완전 LLM 논문이었다. 새로운 분야에 대해 읽었다는 점에 의의를 두자.