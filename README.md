## *VR-AR-Basics of game creation*
### :video_game:&nbsp;&nbsp;**Term Project for 2019**&nbsp;&nbsp;:video_game:
**Team Members**	

\- 박세은 : NPC 대화 구현 및 게임 코드 작성 및 제작				

\- 윤희연 : 장면 전환 구현 및 게임 코드 작성 및 제작				

\- 최애림 : Main Scene 및 전반적인 게임의 틀 제작 및 구현				

**- Title :** 요리의 신


**- 개발 툴 :** Unity 3D

**- 스토리 :** 요리와 관련된 3가지 미니게임을 모두 성공해야만 **요리의 신** 타이틀을 얻을 수 있는 게임				

**- 구성 :**        
<img width="654" alt="image" src="https://user-images.githubusercontent.com/101172040/200779493-ba6c9525-3e57-4891-88df-9461bb9cc172.png">        
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;햄버거 쌓기 게임
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;조합 맞추기 게임
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;잡채 만들기 게임


**- 주요 구현 기능**        

1. 햄버거 쌓기 게임 :


- 배열을 이용하여 사용자가 입력한 재료 순서를 저장하고, 정해 놓은 답이랑 비교하는 코드
	<details>
	<summary>코드</summary>

	``` C
	if(Input.GetKeyDown(KeyCode.Return) // Enter
		{
			if(burger[0] == answer[0] && burger[1] == answer[1] && burger[2] == answer[2]
				&& burger[3] == answer[4] && burger[4] == answer[5] && burger[5] == answer[5])  
			{
				bulgogi_cnt++;
				for(int i = 0 ; i < 4 ; i++)
					{
					burger[i] = 0;
					}
			}
		}
	```
	</details>
           
- 충돌하는 햄버거의 재료를 맨 아래의 빵에 자식으로 설정하여 판별 후 햄버거재료(자식)이 사라지는 코드 구현
	<details>
	<summary>코드</summary>

	``` C
	Public class Colli : MonoBehaviour
	{
		GameObject base bread = null ;
		void Start()
		{
			base bread = GameObject.Find(“base bread”);
		}

		void OnCollisionEnter(Collision coll)
		{
			this.transform.parent = base bread.transform;
		}
		
		...

		foreach(Transform child in transform)
		{
			GameObject.Destroy(child.gameObject);
		}
		//초기화
		i=0;
	}
	```
	</details>
              
2. Canvas를 이용한 NPC 대화 창 및 버튼 클릭 이벤트(장면 전환 등) 코드 구현
	<details>
	<summary>코드</summary>

	``` C
	public class Dialogue
	{
		[TextArea]
		public string dialogue;
		public Sprite cg;
	}

	public class changeScene1 : MonoBehaviour
	{
		[SerializeField] private SpriteRenderer sprite_StandingCG;
		[SerializeField] private SpriteRenderer sprite_DialogueBox;
		[SerializeField] private Text txt_Dialogue;

		private bool isDialogue = false;
		private int cnt = 0;
		[SerializeField] private Dialogue[] dialogue;

		private void OnOff(bool _flag)
		{
			sprite_DialogueBox.gameObject.SetActive(_flag);
			sprite_StandingCG.gameObject.SetActive(_flag);
			txt_Dialogue.gameObject.SetActive(_flag);
			isDialogue(_flag);
		}

		private void NextDialogue()
		{
			txt_Dialogue.text = dialogue[cnt].dialogue;
			sprite_StandingCG.sprite = dialogue[cnt].cg;
			cnt++;
		}

		private void Start()
		{
			cnt = 0;
			isDialogue = true;
		}

		void Update()
		{
			if(isDialogue)
			{
				if(Input.GetKeyDown(KeyCode.Space))
				{
					if(cnt < dialogue.Length)
						NextDialogue();
					else
					{
						OnOff(false);
						SceneManager.LoadScene(1); //장면전환
					}
				}
			}
		}
	}
	```
	</details>


	
              
**- 구현시 어려웠던 점** 
1. 햄버거 게임 : 부모-자식 없애기
2. Main Scene 의 Character 움직임 구현
3. 잡채 만들기 게임 마우스 드래그 구현
