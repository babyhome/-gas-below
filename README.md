# -gas-below
testing gas project


npx tsx example.ts

## สร้างไฟล์ tsconfig.json
`
npx tsc --init
`

```
npm run start
npm run dev -- ./contracts/SimpleStorage.sol
```
* สำคัญ: เครื่องหมาย -- ใช้เพื่อบอก npm ว่าข้อความที่ตามหลัง (./contracts/SimpleStorage.sol) คือ argument ที่จะส่งให้สคริปต์ของเรา ไม่ใช่ของ npm เอง

```
const visitor: ASTVisitor = {
  // โครงสร้างโค้ด
  SourceUnit(node) { /* เข้าถึง source code unit ทั้งหมด */ },
  ContractDefinition(node) { /* เข้าถึงการประกาศ contract */ },
  FunctionDefinition(node) { /* เข้าถึงการประกาศฟังก์ชัน */ },
  ModifierDefinition(node) { /* เข้าถึงการประกาศ modifier */ },
  
  // โครงสร้างข้อมูล
  VariableDeclaration(node) { /* เข้าถึงการประกาศตัวแปร */ },
  StateVariableDeclaration(node) { /* เข้าถึงการประกาศ state variable */ },
  
  // Flow control
  ForStatement(node) { /* เข้าถึง for loop */ },
  WhileStatement(node) { /* เข้าถึง while loop */ },
  DoWhileStatement(node) { /* เข้าถึง do-while loop */ },
  IfStatement(node) { /* เข้าถึง if statement */ },
  
  // การเข้าถึงข้อมูล
  MemberAccess(node) { /* เข้าถึงการใช้งาน member ของ object เช่น a.b */ },
  IndexAccess(node) { /* เข้าถึงการใช้งาน array หรือ mapping เช่น a[i] */ },
  
  // ค่าและนิพจน์
  BinaryOperation(node) { /* เข้าถึง binary operations เช่น a + b, a && b */ },
  Identifier(node) { /* เข้าถึงตัวระบุ identifier เช่น ชื่อตัวแปร */ },
  ArrayTypeName(node) { /* เข้าถึงชนิดข้อมูลแบบ array */ },
  
  // การเรียกฟังก์ชัน
  FunctionCall(node) { /* เข้าถึงการเรียกใช้ฟังก์ชัน */ },
  
  // เพิ่มเติม
  Assignment(node) { /* เข้าถึงการกำหนดค่า */ },
  Return(node) { /* เข้าถึง return statement */ },
};

```
วิธีการใช้งาน Inspector
แต่ละ inspector จะมี node เป็น parameter และคุณสามารถดึงข้อมูลและตรวจสอบคุณสมบัติต่างๆ ได้ ตัวอย่างการตรวจสอบการใช้ state variable ใน loop:

```
const visitor: ASTVisitor = {
  // เก็บข้อมูล state variables
  StateVariableDeclaration(node) {
    node.variables.forEach(variable => {
      if (variable.name) {
        stateVars.add(variable.name);
      }
    });
  },
  
  // ตรวจสอบการใช้ state variables ใน loop
  ForStatement(node) {
    checkStateVarsInNode(node, stateVars, findings, "for loop");
  },
  
  WhileStatement(node) {
    checkStateVarsInNode(node, stateVars, findings, "while loop");
  },
  
  DoWhileStatement(node) {
    checkStateVarsInNode(node, stateVars, findings, "do-while loop");
  },
  
  // ตรวจสอบการใช้ identifier
  Identifier(node) {
    // สามารถตรวจสอบใน context ปัจจุบันว่าอยู่ใน loop หรือไม่
    // และ identifier นั้นเป็น state variable หรือไม่
  }
};

// ฟังก์ชันช่วยตรวจสอบ state vars ในโหนด
function checkStateVarsInNode(node: any, stateVars: Set<string>, findings: Finding[], loopType: string) {
  parser.visit(node, {
    Identifier(idNode) {
      if (stateVars.has(idNode.name)) {
        findings.push({
          rule: "storage-in-loop",
          message: `State variable "${idNode.name}" is accessed inside a ${loopType}`,
          severity: "warning",
          location: idNode.loc
        });
      }
    }
  });
}
```


## For Vitest

```
 npx vitest run
```