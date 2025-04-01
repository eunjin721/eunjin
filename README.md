class FusionReaction:
    def __init__(self, reaction_type, temperature, pressure, time_hours):
        """
        핵융합 반응 초기 설정
        :param reaction_type: 반응 종류 ('D-T' 또는 'D-D')
        :param temperature: 온도 (단위: keV)
        :param pressure: 압력 (단위: atm)
        :param time_hours: 반응이 지속될 시간 (단위: 시간)
        """
        self.reaction_type = reaction_type
        self.temperature = temperature  
        self.pressure = pressure  
        self.time_hours = time_hours  
    
    def calculate_energy_output_per_hour(self):
        """
        시간당 방출 에너지 계산
        :return: 시간당 방출되는 에너지 (단위: MeV/시간)
        """
        if self.reaction_type == "D-T":
            energy_per_reaction = 17.6  
        elif self.reaction_type == "D-D":
            energy_per_reaction = 3.6  
        else:
            return 0  
        
        energy_multiplier = (self.temperature / 10) * (self.pressure / 1)  
        return energy_per_reaction * energy_multiplier  

    def calculate_total_energy(self):
        """
        지정된 시간 동안 방출되는 총 에너지 계산
        :return: 총 방출된 에너지 (단위: MeV)
        """
        return self.calculate_energy_output_per_hour() * self.time_hours  
    
    def is_reaction_possible(self):
        """
        반응이 가능한 온도 및 압력 조건 확인
        :return: 가능 여부 (True/False)
        """
        return self.temperature >= 10 and self.pressure >= 1
    
    def __str__(self):
        """
        반응 정보를 문자열로 변환
        """
        status = "가능" if self.is_reaction_possible() else "불가능"
        total_energy_mev = self.calculate_total_energy() if self.is_reaction_possible() else 0
        return (f"반응 종류: {self.reaction_type}\n"
                f"온도: {self.temperature} keV, 압력: {self.pressure} atm\n"
                f"반응 가능 여부: {status}\n"
                f"시간당 방출 에너지: {self.calculate_energy_output_per_hour()} MeV/시간\n"
                f"지정된 시간 동안 방출된 총 에너지: {total_energy_mev} MeV")

def fusion_simulation():
    """
    핵융합 반응 시뮬레이션 실행 함수
    """
    while True:
        try:
            reaction_type = input("반응 종류를 입력하세요 (D-T 또는 D-D): ").strip()
            if reaction_type not in ["D-T", "D-D"]:
                raise ValueError("올바른 반응 종류를 입력하세요 (D-T 또는 D-D).")
            
            temperature = float(input("온도를 입력하세요 (단위: keV): "))
            if temperature <= 0:
                raise ValueError("온도는 0보다 커야 합니다.")
            
            pressure = float(input("압력을 입력하세요 (단위: atm): "))
            if pressure <= 0:
                raise ValueError("압력은 0보다 커야 합니다.")
            
            time_hours = float(input("반응이 지속될 시간을 입력하세요 (단위: 시간): "))
            if time_hours <= 0:
                raise ValueError("시간은 0보다 커야 합니다.")
            
            break  
        except ValueError as e:
            print(f"입력 오류: {e} 다시 입력하세요.")

    reaction = FusionReaction(reaction_type, temperature, pressure, time_hours)
    print("\n결과:")
    print(reaction)

if __name__ == "__main__":
    fusion_simulation()
