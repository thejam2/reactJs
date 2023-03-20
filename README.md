리액트 메모 

리액트 프로젝트 생성
npx create-react-app 프로젝트명 --template typescript
리액트 라우터 설치
npm install react-router-dom
스타일 컴포넌트 설치
npm install styled-components
Recoil 설치
npm install recoil


styled-component

기본형식
const Box = styled.div`
  background-color: ${(props) => props.bgColor};
  width: 100px;
  height: 100px;
`;
상속느낌
const Circle = styled(Box)`
  border-radius: 50px;
`;
html변경 원할시 as추가
<Father as="header">
공통 attr 추가시
const Input = styled.input.attrs({ required: true })`
  background-color: tomato;
`;
애니메이션
const rotationAnimation = keyframes`
  0% {
    transform:rotate(0deg);
    border-radius:0px;
  }
  50% {
    border-radius:100px;
  }
  100%{
    transform:rotate(360deg);
    border-radius:0px;
  }
  
  //or
  
  from {
    transform:rotate(0deg);
    border-radius:0px;
  }
  to{
    transform:rotate(360deg);
    border-radius:0px;
  }
`;
const Box = styled.div`
  height: 200px;
  width: 200px;
  background-color: tomato;
  display: flex;
  justify-content: center;
  align-items: center;
  animation: ${rotationAnimation} 1s linear infinite;	//애니메이션
  span {	//Box 내부 타겟
    font-size: 36px;
    &:hover {	// &은 span가리킴 (span:hover와 동일)
      font-size: 48px;
    }
    &:active {
      opacity: 0;
    }
  }
`;
컴포넌트 선택시
${Emoji}:hover {
    font-size: 98px;
}
  



타입스크립트
js기반으로 타입체크 필요
const plus = (a:number, b:number) => a+b;

인터페이스는 props 타입 설정
interface CircleProps {
    bgColor: string;
    borderColor? : string;	//?는 required를 option으로 변경해줌
	text?: string;
}

function Circle({ bgColor, borderColor, text = "default text" }: CircleProps) {	//Circle(props:CircleProps) 도 가능, text="디폴트값"
	const [value, setValue] = useState<string>("");	//타입스크립트는 초기값의 타입으로 value, setValue도 같은타입으로 추측함, 변경원할시 <string|number> 이런식
    return (
        <Container bgColor={bgColor} borderColor={borderColor ?? bgColor}>	// ??는 값이 undefined일시 default값 설정
            {text}
        </Container>
    );
}

const onChange = (e: React.FormEvent<HTMLInputElement>) => {	//리액트에서 이벤트 발생방법 React.이벤트이름<이벤트발생시키는Element>
    const {	//ES6 문법
      currentTarget: { value },
    } = e;
    setValue(value);
  };
  
  
 리액트쿼리
 첫번째 파라미터 = 유니크 키
 두번째 파라미터 = 비동기 함수(api호출 함수) (당연한 말이지만 두번째 파라미터는 promise가 들어가야합니다.)
 세번째 파라미터 = 
 
 
 Recoil			-> 상태관리라이브러리
 atom이라는걸 사용
 atom.ts 파일에서 함수 생성
 export const isDarkAtom = atom({
  key: 키 명,
  default: 값,
});

export const toDoSelector = selector({		//selector로 atom과 연결 가능
  key: 키 명,
  get: ({ get }) => {
    const toDos = get(atom 이름);
    const category = get(atom 이름);
    return toDos.filter((toDo) => toDo.category === category);
  },
  set: ({ set }, newValue) => {
        const minutes = Number(newValue) * 60;
        set(minuteState, minutes);
		//set(세팅할atom명, 세팅할 값);
    },
});

 필요한 파일에서 호출
 const isDark = useRecoilValue(isDarkAtom); // atom값 가져옴
 const setDarkAtom = useSetRecoilState(isDarkAtom);	// set atom 함수 가져옴 (useState의 set같음)
const [toDos, setToDos] = useRecoilState(toDoState);	// recoil state처럼 사용가능
const [hours, setHours] = useRecoilState(hourSelector);	//()안이 selector 면 첫번쨰값은 get, 두번째값은 set

enum
export enum Categories {	//Categories.TO_DO 값 0,       1, 2 ....
  "TO_DO",
  "DOING",
  "DONE",
}

export enum Categories {	// Categories.TO_DO 값 TO_DO (지정해준 이름)
  "TO_DO" = "TO_DO",
  "DOING" = "DOING",
  "DONE" = "DONE",
}

 
 
 react-hook-form			-> https://react-hook-form.com/
 const { register, handleSubmit, formState } = useForm()
 const { register, handleSubmit, formState: { errors }, watch } = useForm<IForm>({		//<>는 타입스크립트 인터페이스
        defaultValues: {	//초기 디폴트 값
            email: "@naver.com",
        }
    });
 
 <form	onSubmit={handleSubmit(onValid)}	// form태그의 OnSubmit에 handleSubmit(서브밋 후 함수) 
				
<input
                    {...register("password", { required: true, minLength: 5 })}
                    placeholder="Password"
                />
<input
                    {...register("password1", {
                        required: "Password is required",
                        minLength: {
                            value: 5,
                            message: "Your password is too short.",
                        },
                    })}
                    placeholder="Password1"
                />	//위, 아래 유형 둘다 가능
<span>{errors?.password1?.message}</span>	//에러 메세지 표현
 
 register: (name: string, RegisterOptions?) => ({ onChange, onBlur, name, ref })
이 메서드를 사용하면 input을 등록하거나 element를 선택하고 유효성 검사 규칙을 React Hook Form에 적용할 수 있습니다.
유효성 검사 규칙은 모두 HTML 표준을 기반으로 하며 사용자 지정 유효성 검사 방법도 허용합니다.

watch: (names?: string | string[] | (data, options) => void) => unknown
input의 변화를 구독합니다. 이 메서드는 지정된 input을 감시하고 해당 값을 반환합니다. input 값을 렌더링하고 조건에 따라 무엇을 렌더링할지 결정하는 데 유용합니다.

handleSubmit : 이 함수는 양식 유효성 검사가 성공하면 양식 데이터를 수신합니다.

formState : 이 개체에는 전체 양식 상태에 대한 정보가 포함되어 있습니다. 양식 응용 프로그램과 사용자의 상호 작용을 추적하는 데 도움이 됩니다.

setError : 발생하는 문제에 따라 추가적으로 에러 설정

setValue : submit후 value 세팅



animation		framer-motion
npm i framer-motion
<motion.태그명></motion.태그명>	//그냥 태그에 사용시

const Box = styled(motion.div)``;	//styled-component에 사용시
<Box
        transition={{ type: "spring", delay: 0.5 }}
        initial={{ scale: 0 }}
        animate={{ scale: 1, rotateZ: 360 }}
      />
	  

variants
const circleVariants = {
  start: {
    opacity: 0,
    y: 10,
  },
  end: {
    opacity: 1,
    y: 0,
  },
};

<Box variants={boxVariants} initial="start" animate="end">	//부모에 initial, animate 있을시 자식에도 자동으로 부여 (이름 같을때)
        <Circle variants={circleVariants} />
        <Circle variants={circleVariants} />
        <Circle variants={circleVariants} />
        <Circle variants={circleVariants} />
</Box>

<Box whilehover={{scale:1.5, rotateZ:90}} whileTap={{scale:1, borderRadius:"100px"}}>	// whilehover : 마우스 올렸을때, whileTap: 마우스 클릭시\
variants 사용시 
const boxVariants = {
  hover: { scale: 1.5, rotateZ: 90 },
  click: { scale: 1, borderRadius: "100px" },
  drag: { backgroundColor: "rgb(46, 204, 113)", transition: { duration: 10 } },
};
<Box
          drag												//drag 추가시 드래그 가능	(drag="x"는 x축만이동가능)
          dragSnapToOrigin									//드래그 후 제자리 이동
          dragElastic={0.5}									//드래그 탄성(0~1사이)
          dragConstraints={biggerBoxRef}					//드래그 범위
          variants={boxVariants}
          whileHover="hover"
          whileTap="click"
        />								

const x = useMotionValue(0);	//	useMotionValue 이동할때마다 값 가져옴
const scale = useTransform(x, [-800, 0, 800], [2, 1, 0.1]);	//useTransform은 이동할떄 원하는값 가져옴 첫번째값:기준, 두번째값 : 기준값 범위, 세번째값 : 두번째값에따라 원하는값
const { scrollY, scrollYProgress } = useViewportScroll();	//useViewportScroll은 스크롤에 따른 값 가져옴 scrollY는 스크롤한 픽셀값, scrollYProgress는 스크롤한 비율
<Box style={{ x, rotateZ, scale }} drag="x" dragSnapToOrigin />

SVG 애니메이션
const Svg = styled.svg`
  width: 300px;
  height: 300px;
  path {						//라인
    stroke: white;	
    stroke-width: 2;
  }
`;

const svg = {	//fill은 색 채우기, pathLength는 라인 그리기
  start: { pathLength: 0, fill: "rgba(255, 255, 255, 0)" },
  end: {
    fill: "rgba(255, 255, 255, 1)",			//색 채우기
    pathLength: 1,	
  },
};
<Svg
        focusable="false"
        xmlns="http://www.w3.org/2000/svg"
        viewBox="0 0 448 512"
      >
        <motion.path
          variants={svg}
          initial="start"
          animate="end"
          transition={{	//기본과 속성 따로 설정하고 싶을땐 prop으로 작성
            default: { duration: 5 },	//디폴트 지연 5
            fill: { duration: 1, delay: 3 },	//fill할때만 따로 설정
          }}
          d="M224 373.12c-25.24-31.67-40.08-59.43-45-83.18-22.55-88 112.61-88 90.06 0-5.45 24.25-20.29 52-45 83.18zm138.15 73.23c-42.06 18.31-83.67-10.88-119.3-50.47 103.9-130.07 46.11-200-18.85-200-54.92 0-85.16 46.51-73.28 100.5 6.93 29.19 25.23 62.39 54.43 99.5-32.53 36.05-60.55 52.69-85.15 54.92-50 7.43-89.11-41.06-71.3-91.09 15.1-39.16 111.72-231.18 115.87-241.56 15.75-30.07 25.56-57.4 59.38-57.4 32.34 0 43.4 25.94 60.37 59.87 36 70.62 89.35 177.48 114.84 239.09 13.17 33.07-1.37 71.29-37.01 86.64zm47-136.12C280.27 35.93 273.13 32 224 32c-45.52 0-64.87 31.67-84.66 72.79C33.18 317.1 22.89 347.19 22 349.81-3.22 419.14 48.74 480 111.63 480c21.71 0 60.61-6.06 112.37-62.4 58.68 63.78 101.26 62.4 112.37 62.4 62.89.05 114.85-60.86 89.61-130.19.02-3.89-16.82-38.9-16.82-39.58z"
        ></motion.path>
      </Svg>
	  
	  
AnimatePresence
AnimatePresence는 안에 조건문, visible상태여야 함
const box = {
  entry: {
    x: 500,
    opacity: 0,
    scale: 0,
  },
  center: {
    x: 0,
    opacity: 1,
    scale: 1,
    transition: {
      duration: 1,
    },
  },
  exit: { x: -500, opacity: 0, scale: 0, transition: { duration: 1 } },
};

<AnimatePresence custom={back} onExitComplete={함수명} initial={false}>		//onExitComplete은 exit끝나고 실행, initial={false}는 맨 처음은 initial 안함
        <Box
          custom={back}
          variants={box}
          initial="entry"
          animate="center"
          exit="exit"
          key={visible}
        >
          {visible}
        </Box>
      </AnimatePresence>		//[위 객체와 이름 같아야함] initial나타날떄(시작), animate애니메이션 중(중간), exit 사라질때(끝), custom 사용시 아래 객체처럼 사용(파라미터 전달가능)
	  
	  
	  const box = {
  entry: (isBack:boolean) => (
    {
      x: isBack? -500 : 500,
      opacity: 0,
      scale: 0,
    }
  ),
  center: {
    x: 0,
    opacity: 1,
    scale: 1,
    transition: {
      duration: 1,
    },
  },
  exit: (isBack:boolean) => (
    { x: isBack? 500 : -500, opacity: 0, scale: 0, transition: { duration: 1 } }
  ),
};


layout		//layout속성 주면 애니메이션 됨, layoutId 동일하게 주면 같은 컴포넌트로 인식하고 애니메이션 됨
<Wrapper onClick={toggleClicked}>
      <Box>
        {!clicked ? (
          <Circle layoutId="circle" style={{ borderRadius: 50 }} />
        ) : null}
      </Box>
      <Box>
        {clicked ? (
          <Circle layoutId="circle" style={{ borderRadius: 0, scale: 2 }} />
        ) : null}
      </Box>
    </Wrapper>
	
	
useMatch("경로"); 	// 경로에 있는지 확인 true or false 리턴

transition={{ type: "linear" }}		//linear은 동시에 작동

코드에서 animation 필요시
const inputAnimation = useAnimation();	//useAnimation() 선언후 변수명.start 사용, 태그에는 animate={변수명}
inputAnimation.start({
        scaleX: 0,
      });
	  

//styled-component에 변수 전달 방법
const Box = styled(motion.div) <{ bgPhoto: string }>`
  background-image: url(${(props) => props.bgPhoto});
`;
<Box bgPhoto={makeImagePath(movie.backdrop_path, "w500")} />



react-beautiful-dnd	//드래그 가능하게 해주는 라이브러리				현재 React18 이슈가 있으니 index에서 스트릭트모드를 제거하시거나 그냥 이런게 있다 정도로만 보시면 될 것 같습니다.
<DragDropContext onDragEnd={onDragEnd}>
      <div>
        <Droppable droppableId="one">
          {(magic) => (
            <ul ref={magic.innerRef} {...magic.droppableProps}>
              <Draggable draggableId="first" index={0}>
                {(magic) => (
                  <li ref={magic.innerRef} {...magic.draggableProps}>
                    <span {...magic.dragHandleProps}>🔥</span>
                    One
                  </li>
                )}
              </Draggable>
            </ul>
          )}
        </Droppable>
      </div>
    </DragDropContext>


{magic.placeholder}
export default React.memo(DragabbleCard);
splice
<Droppable droppableId={boardId}>
        {(magic, info) => (
          <Area
            isDraggingOver={info.isDraggingOver}
            isDraggingFromThis={Boolean(info.draggingFromThisWith)}
            ref={magic.innerRef}
            {...magic.droppableProps}
          >
            {toDos.map((toDo, index) => (
              <DragabbleCard key={toDo} index={index} toDo={toDo} />
            ))}
            {magic.placeholder}
          </Area>
        )}
      </Droppable>
	  
	  <Draggable draggableId={toDo} index={index}>
            {(magic, snapshot) => (
                <Card
                    isDragging={snapshot.isDragging}
                    ref={magic.innerRef}
                    {...magic.dragHandleProps}
                    {...magic.draggableProps}
                >
                    {toDo}
                </Card>
            )}
        </Draggable>
		

		
		
